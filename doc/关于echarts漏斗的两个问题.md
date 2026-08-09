Q:第一个问题是漏斗显示,初版显示为上层矩形下层梯形,而期望上层梯形中层矩形下层梯形,后面被"修复"

A:实际不是问题,每个漏斗映射到一个数组(抽象),数组元素个数决定了该层形状,若想修改直接改数据库内元组数量即可

Q:前端echarts在我的版本显示失败,查看页面源代码有

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>企航CRM｜企业轻量客户资源管理系统</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/admin-lte@3.2/dist/css/adminlte.min.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.4/css/all.min.css">
<link rel="stylesheet" href="/crm-pro/static/style.css">
</head>
<body class="hold-transition sidebar-mini layout-fixed">
	<div class="wrapper">
<nav class="main-header navbar navbar-expand navbar-white navbar-light">
	<ul class="navbar-nav">
		<li class="nav-item">
			<a class="nav-link" data-widget="pushmenu" href="#">
				<i class="fas fa-bars"></i>
			</a>
		</li>
		<li class="nav-item d-none d-sm-inline-block">
			<a href="/crm-pro/dashboard" class="nav-link">首页</a>
		</li>
	</ul>
	<ul class="navbar-nav ml-auto">
		<li class="nav-item">
			<a href="/crm-pro/warning/list" class="nav-link">
				<i class="fas fa-exclamation-triangle text-warning"></i>
				
					<span class="badge badge-danger navbar-badge">2</span>
				
			</a>
		</li>
		<li class="nav-item dropdown">
			<a class="nav-link" data-toggle="dropdown" href="#">
				<i class="far fa-user mr-1"></i>
				系统管理员
				<span class="badge badge-info ml-1">管理员</span>
			</a>
			<div class="dropdown-menu dropdown-menu-right">
				<div class="dropdown-item-text text-muted small">admin</div>
				<div class="dropdown-divider"></div>
				<a href="/crm-pro/logout" class="dropdown-item">
					<i class="fas fa-sign-out-alt mr-2"></i>
					退出登录
				</a>
			</div>
		</li>
	</ul>
</nav>

<aside class="main-sidebar sidebar-dark-primary elevation-4">
	<a href="/crm-pro/dashboard" class="brand-link">
		<span class="brand-mark">Q</span>
		<span class="brand-text font-weight-semibold">企航CRM</span>
	</a>
	<div class="sidebar">
		<div class="user-panel mt-3 pb-3 mb-3 d-flex">
			<div class="image">
				<span class="user-avatar">
					<i class="fas fa-user"></i>
				</span>
			</div>
			<div class="info">
				<span class="d-block text-white">
					系统管理员
				</span>
				<small class="text-muted">管理员</small>
			</div>
		</div>
		<nav class="mt-2">
			<ul class="nav nav-pills nav-sidebar flex-column" data-widget="treeview">
				<li class="nav-header">工作台</li>
				<li class="nav-item">
					<a href="/crm-pro/dashboard" class="nav-link active">
						<i class="nav-icon fas fa-chart-pie"></i>
						<p>数据仪表盘</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/customer/list" class="nav-link ">
						<i class="nav-icon fas fa-building"></i>
						<p>客户管理</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/contact/list" class="nav-link ">
						<i class="nav-icon fas fa-address-book"></i>
						<p>联系人管理</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/product/list" class="nav-link ">
						<i class="nav-icon fas fa-cubes"></i>
						<p>产品管理</p>
					</a>
				</li>
				<li class="nav-header">销售过程</li>
				<li class="nav-item">
					<a href="/crm-pro/opportunity/list" class="nav-link ">
						<i class="nav-icon fas fa-chart-line"></i>
						<p>商机管理</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/follow/list" class="nav-link ">
						<i class="nav-icon fas fa-clipboard-list"></i>
						<p>跟进记录</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/contract/list" class="nav-link ">
						<i class="nav-icon fas fa-file-contract"></i>
						<p>合同管理</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/funnel" class="nav-link ">
						<i class="nav-icon fas fa-filter"></i>
						<p>销售漏斗</p>
					</a>
				</li>
				<li class="nav-item">
					<a href="/crm-pro/warning/list" class="nav-link ">
						<i class="nav-icon fas fa-exclamation-triangle text-warning"></i>
						<p>
							流失预警
							
								<span class="right badge badge-danger">2</span>
							
						</p>
					</a>
				</li>
			</ul>
		</nav>
	</div>
</aside>
<div class="content-wrapper">
			<div class="content-header">
				<div class="container-fluid">
					<div class="d-flex justify-content-between align-items-center">
						<div>
							<h1>数据仪表盘</h1>
							<p class="text-muted mb-0">欢迎回来，系统管理员。这里是今天的销售经营概览。</p>
						</div>
						<a href="/crm-pro/follow/add" class="btn btn-primary">
							<i class="fas fa-plus mr-1"></i>
							记录跟进
						</a>
					</div>
				</div>
			</div>
			<section class="content">
				<div class="container-fluid">
<div class="row">
						<div class="col-lg-2 col-md-4 col-6">
							<div class="small-box bg-info">
								<div class="inner">
									<h3>4</h3>
									<p>客户总数</p>
								</div>
								<div class="icon">
									<i class="fas fa-building"></i>
								</div>
								<a href="/crm-pro/customer/list" class="small-box-footer">
									查看客户
									<i class="fas fa-arrow-circle-right"></i>
								</a>
							</div>
						</div>
						<div class="col-lg-2 col-md-4 col-6">
							<div class="small-box bg-success">
								<div class="inner">
									<h3>1</h3>
									<p>本月新增</p>
								</div>
								<div class="icon">
									<i class="fas fa-user-plus"></i>
								</div>
								<span class="small-box-footer">持续拓展客户池</span>
							</div>
						</div>
						<div class="col-lg-2 col-md-4 col-6">
							<div class="small-box bg-warning">
								<div class="inner">
									<h3>5</h3>
									<p>进行中商机</p>
								</div>
								<div class="icon">
									<i class="fas fa-chart-line"></i>
								</div>
								<a href="/crm-pro/opportunity/list" class="small-box-footer">
									查看商机
									<i class="fas fa-arrow-circle-right"></i>
								</a>
							</div>
						</div>
						<div class="col-lg-2 col-md-4 col-6">
							<div class="small-box bg-danger">
								<div class="inner">
									<h3>
										<small>¥</small>
										214,600
									</h3>
									<p>预计成交金额</p>
								</div>
								<div class="icon">
									<i class="fas fa-yen-sign"></i>
								</div>
								<span class="small-box-footer">进行中商机总额</span>
							</div>
						</div>
						<div class="col-lg-2 col-md-4 col-6">
							<div class="small-box bg-secondary">
								<div class="inner">
									<h3>2</h3>
									<p>今日待跟进</p>
								</div>
								<div class="icon">
									<i class="fas fa-calendar-check"></i>
								</div>
								<span class="small-box-footer">及时完成计划</span>
							</div>
						</div>
						<div class="col-lg-2 col-md-4 col-6">
							<div class="small-box bg-danger">
								<div class="inner">
									<h3>2</h3>
									<p>高风险客户</p>
								</div>
								<div class="icon">
									<i class="fas fa-exclamation-triangle"></i>
								</div>
								<a href="/crm-pro/warning/list" class="small-box-footer">
									立即处理
									<i class="fas fa-arrow-circle-right"></i>
								</a>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="col-lg-7">
							<div class="card">
								<div class="card-header">
									<h3 class="card-title">
										<i class="fas fa-filter text-primary mr-2"></i>
										销售漏斗概览
									</h3>
									<a href="/crm-pro/funnel" class="float-right">查看详情</a>
								</div>
								<div class="card-body">
									
									
										
										
											<div id="funnel" class="funnel-container"></div>
										
									
								</div>
							</div>
						</div>
						<div class="col-lg-5">
							<div class="card">
								<div class="card-header">
									<h3 class="card-title">
										<i class="fas fa-bell text-warning mr-2"></i>
										今日待跟进
									</h3>
								</div>
								<div class="card-body p-0">
									<div class="table-responsive">
										<table class="table mb-0">
											<thead>
												<tr>
													<th>时间</th>
													<th>客户</th>
													<th>计划</th>
													<th></th>
												</tr>
											</thead>
											<tbody>
												
													<tr class="table-warning">
														<td>2026-08-09 10:30</td>
														<td>
															青禾商业管理有限公司
														</td>
														<td>
															电话确认需求
														</td>
														<td>
															<a href="/crm-pro/follow/add?customerId=4" class="btn btn-xs btn-primary">跟进</a>
														</td>
													</tr>
												
													<tr class="table-warning">
														<td>2026-08-09 15:00</td>
														<td>
															星海科技有限公司
														</td>
														<td>
															发送正式合同草案
														</td>
														<td>
															<a href="/crm-pro/follow/add?customerId=1" class="btn btn-xs btn-primary">跟进</a>
														</td>
													</tr>
												
												
											</tbody>
										</table>
									</div>
								</div>
							</div>
						</div>
					</div>
					<div class="card">
						<div class="card-header">
							<h3 class="card-title">
								<i class="fas fa-history text-success mr-2"></i>
								最近跟进记录
							</h3>
						</div>
						<div class="card-body p-0">
							<div class="table-responsive">
								<table class="table mb-0">
									<thead>
										<tr>
											<th>跟进时间</th>
											<th>客户</th>
											<th>商机</th>
											<th>方式</th>
											<th>跟进内容</th>
											<th>跟进人</th>
										</tr>
									</thead>
									<tbody>
										
											<tr>
												<td>2026-08-07 09:57</td>
												<td>
													星海科技有限公司
												</td>
												<td>
													星海 CRM 升级项目
												</td>
												<td>
													<span class="badge badge-info">线上会议</span>
												</td>
												<td>
													确认最终实施范围与排期
												</td>
												<td>销售专员</td>
											</tr>
										
											<tr>
												<td>2026-08-02 09:57</td>
												<td>
													青禾商业管理有限公司
												</td>
												<td>
													青禾客户经营平台
												</td>
												<td>
													<span class="badge badge-info">邮件</span>
												</td>
												<td>
													发送产品介绍与案例
												</td>
												<td>李四</td>
											</tr>
										
											<tr>
												<td>2026-07-28 09:57</td>
												<td>
													云帆教育服务有限公司
												</td>
												<td>
													云帆销售流程数字化
												</td>
												<td>
													<span class="badge badge-info">电话</span>
												</td>
												<td>
													了解销售团队规模和流程
												</td>
												<td>李四</td>
											</tr>
										
											<tr>
												<td>2026-07-04 09:57</td>
												<td>
													远峰智能制造集团
												</td>
												<td>
													远峰智能驾驶舱项目
												</td>
												<td>
													<span class="badge badge-info">拜访</span>
												</td>
												<td>
													完成驾驶舱原型演示
												</td>
												<td>销售专员</td>
											</tr>
										
									</tbody>
								</table>
							</div>
						</div>
					</div>
				</div>
			</section>
		</div><footer class="main-footer">
	<div class="float-right d-none d-sm-inline">企航CRM v1.0</div>
	<strong>Copyright &copy; 2026</strong>
	企航CRM
</footer>
</div><script src="https://cdn.jsdelivr.net/npm/jquery@3.6/dist/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6/dist/js/bootstrap.bundle.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/admin-lte@3.2/dist/js/adminlte.min.js"></script>
<script>if(location.pathname.includes('/contract/list')){document.querySelectorAll('table tbody tr').forEach(r=>{if(r.cells.length<8||!r.cells[6].innerText.includes('执行中'))return;const end=new Date(r.cells[5].innerText.trim()+'T00:00:00'),now=new Date();now.setHours(0,0,0,0);const days=(end-now)/86400000;if(days>=0&&days<=30)r.classList.add('table-warning')})}</script>
<script src="https://cdn.jsdelivr.net/npm/echarts@5.4/dist/echarts.min.js"></script>
	<script>
		if (typeof echarts === 'undefined') {
			document
					.write('<script src="https://cdn.bootcdn.net/ajax/libs/echarts/5.4.3/echarts.min.js"><\/script>');
		}
	</script>
	<script src="/crm-pro/static/funnel-chart.js"></script>
	
		<script>
			(function() {
				var d = $
				{
					funnelJson
				}
				;
				QihangFunnel.create(document.getElementById('funnel'), d);
			})();
		</script>
	
</body>
</html>
```

浏览器的开发人员工具提示

```
dashboard:435 Uncaught ReferenceError: funnelJson is not defined
    at dashboard:435:6
    at dashboard:439:6
```

死因:

看最后几行的

```
<script>
	(function() {
		var d = $
		{
			funnelJson
		}
		;
		QihangFunnel.create(document.getElementById('funnel'), d);
	})();
</script>
```

jsp只把同一行连续的\${...}识别为EL表达式并求值,如果$与大括号换行,它就退化为纯文本原样输出给浏览器,结果浏览器拿到无效js,把funnelJson当成未定义js变量,于是报如上错误

dashboard.jsp和funnel_chart.jsp都有这两个情况

合法的情况应该是

```
<script>
	(function()
    {
		var d = ${funnelJson};
		QihangFunnel.create(document.getElementById('funnel'), d);
	})();
</script>
```

根因:我去用了格式化器/_ \格式化器直接把这玩意拆开了

别论:

我之前的jsp压缩器会在这里出现问题吗?

不会,因为那是压缩,不是分行,不过也提醒一点就是EL表达式是依赖于源代码字符的,但是我的压缩器是把换行压成空格,合法的EL表达式不会出现损失

可推知,如果有一个同类的表面不合法的"EL表达式"是其他合法用途,压缩后会变成EL表达式同时造成语义损失,jsp压缩器压缩的jsp应该避免这种情况,参见
https://github.com/haidaomei/Personal-Formatter/blob/main/JSPCompression/README.md