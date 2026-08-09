# 领航 CRM — 业务 CRUD 设计文档

> 本文档描述各数据表的增删改查业务逻辑，聚焦多表联查与多表写入场景。
> 三张字典表（`crm_customer_level`、`crm_lead_source`、`crm_opportunity_stage`）为只读数据，不在此讨论。

---

## CRUD 总览

| 模块 | 主表 | 增 | 删 | 改 | 查(多表) | 涉及多表写入 |
|------|------|:--:|:--:|:--:|:--:|:--:|
| 注册 | `sys_user` | ✓ | — | — | — | |
| 客户 | `crm_customer` | ✓ | ✓ | ✓ | ✓ | 转移时 |
| 联系人 | `crm_contact` | ✓ | ✓ | ✓ | — | |
| 产品 | `crm_product` | ✓ | ✓ | ✓ | — | |
| 商机 | `crm_business_opportunity` | ✓ | ✓ | ✓ | ✓ | 主从表 + 推进触发跟进 |
| 跟进 | `crm_follow_up_record` | ✓ | ✓ | — | — | 新增时更新客户时间 |
| 合同 | `crm_contract` | ✓ | ✓ | ✓ | ✓ | 主从表 |

---

## 1. 注册 — `sys_user`

| 操作 | 说明 |
|------|------|
| **增** | 纯 INSERT，不涉及其他表 |

---

## 2. 客户 — `crm_customer`

| 操作 | 依赖表（读取表单数据） | 被依赖表（同时写入/级联隐藏/详情读取） | 说明 |
|------|----------------------|-------------------------------------|------|
| **增** | `sys_user` | — | 表单提供负责人下拉选项 |
| **删** | — | 级联不显示：`crm_contact`、`crm_business_opportunity`、`crm_follow_up_record`、`crm_contract`、`crm_customer_transfer_log` | 逻辑删除后关联数据不再展示 |
| **改** | `sys_user` | — | 同增 |
| **查** | `sys_user` | 调用被依赖表：`crm_contact`、`crm_business_opportunity`、`crm_follow_up_record`、`crm_customer_transfer_log` | 详情页聚合展示，不调用 `crm_contract` |

---

## 3. 联系人 — `crm_contact`

| 操作 | 依赖表（读取表单数据） | 被依赖表（级联隐藏） | 说明 |
|------|----------------------|-------------------|------|
| **增** | `crm_customer` | — | 表单提供所属客户下拉选项 |
| **删** | — | 级联不显示：`crm_business_opportunity`、`crm_follow_up_record` | 逻辑删除后关联商机和跟进不再展示 |
| **改** | `crm_customer` | — | 同增 |

---

## 4. 产品 — `crm_product`

| 操作 | 依赖表 | 被依赖表（级联隐藏） | 说明 |
|------|--------|-------------------|------|
| **增** | — | — | 独立实体，无依赖 |
| **删** | — | 级联不显示：`crm_opportunity_product`、`crm_contract_product` | 逻辑下架后关联明细不再展示 |
| **改** | — | — | 独立实体 |

---

## 5. 商机 — `crm_business_opportunity`

| 操作 | 依赖表（读取表单数据） | 被依赖表（同时写入/级联隐藏/详情读取） | 说明 |
|------|----------------------|-------------------------------------|------|
| **增** | `sys_user`、`crm_customer`、`crm_contact` | 调用被依赖表：`crm_opportunity_product` | 表单提供负责人、客户下拉和联系人联动；同时写入产品明细 |
| **删** | — | 级联不显示：`crm_follow_up_record`、`crm_opportunity_product`、`crm_contract` | 逻辑删除后跟进、明细、合同不再展示 |
| **改** | `sys_user`、`crm_customer`、`crm_contact` | 普通编辑写入：`crm_opportunity_product` | 表单同增； |
| | | 推进阶段(`/advance`)额外写入：`crm_follow_up_record`（自动生成系统跟进）+ `crm_customer`（更新 `last_follow_time`） | 商机改(推进)会造成跟进增，两者包裹在同一事务中 |
| | | 推进至"已成交"后可手动触发合同增（见第 7 节） | 合同增独立于商机改的流程，但由商机改触发 |
| **查** | `sys_user`、`crm_customer`、`crm_contact` | 调用被依赖表：`crm_follow_up_record`、`crm_opportunity_product` | 详情页聚合展示跟进记录和产品明细 |

---

## 6. 跟进 — `crm_follow_up_record`

| 操作 | 依赖表（读取表单数据） | 被依赖表（同时写入） | 说明 |
|------|----------------------|-------------------|------|
| **增** | `crm_customer`、`crm_contact`、`crm_business_opportunity`（有时不显式） | `crm_customer`（更新 `last_follow_time`） | 客户必选，商机和联系人可选；同时更新客户最后跟进时间 |
| **删** | — | — | 叶子节点，无级联影响 |

> 跟进增还存在另一种触发方式：商机推进时由系统自动生成（`follow_type='系统记录'`），此时为不显式增，由商机改触发。

---

## 7. 合同 — `crm_contract`

| 操作 | 依赖表（读取表单数据） | 被依赖表（同时写入/级联隐藏/详情读取） | 说明 |
|------|----------------------|-------------------------------------|------|
| **增** | `sys_user`、`crm_customer`、`crm_business_opportunity`（有时不显式） | 调用被依赖表：`crm_contract_product` | 可从已成交商机预填生成（`opportunityId` 为 hidden），也可手工创建；同时写入产品明细 |
| **删** | — | 级联不显示：`crm_contract_product` | 逻辑删除后明细不再展示 |
| **改** | `crm_customer` | 调用被依赖表：`crm_contract_product` | 商机关联在生成时固化，不可后续更改 |
| **查** | `sys_user`、`crm_customer`、`crm_business_opportunity`（有时） | 调用被依赖表：`crm_contract_product` | 详情页展示合同信息、关联客户与产品明细 |

---

## 关系速查：谁被谁级联

以下从"删了一个实体后哪些关联数据不再展示"的角度汇总：

| 删除 | 影响范围 |
|------|---------|
| `crm_customer` | contact、opportunity、follow_up、contract、transfer_log |
| `crm_contact` | opportunity、follow_up |
| `crm_product` | opportunity_product、contract_product |
| `crm_business_opportunity` | opportunity_product、follow_up、contract |
| `crm_contract` | contract_product |
| `crm_follow_up_record` | （无，叶子节点） |

> 注：所有"删除"均为逻辑删除（status=0），不物理删除数据库记录。

---

## 事务要点总结

| 场景 | 涉及表 | 说明 |
|------|--------|------|
| **商机主从表增/改** | opportunity + opportunity_product | 主从表写入必须在同一事务 |
| **合同主从表增/改** | contract + contract_product | 主从表写入必须在同一事务 |
| **客户转移** | customer + transfer_log | 更新负责人 + 写入转移日志在同一事务 |
| **跟进增** | follow_up + customer | 插入跟进 + 更新客户最后跟进时间在同一事务 |
| **商机推进** | opportunity + follow_up + customer | 更新阶段 + 生成系统跟进 + 更新客户时间在同一事务 |

> 跟进和合同在某些时刻依赖于商机修改而生成，它们的不显式增对应着商机的改。两者流程独立于商机改，但都从商机改（推进阶段）触发。
