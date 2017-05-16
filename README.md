#idleaf

##简介


对于 [http://tech.meituan.com/MT_Leaf.html?utm_source=tuicool&utm_medium=referral](http://tech.meituan.com/MT_Leaf.html?utm_source=tuicool&utm_medium=referral "Leaf——美团点评分布式ID生成系统") Leaf——美团点评分布式ID生成系统 中介绍的
Leaf-segment数据库方案 生成唯一orderId的方案的一个实现。
在实现中使用双buffer优化，在第一个buffer使用50%的时候去加载另一个buffer的数据，这里分同步与异步两种方式，默认是同步加载。对于异步增加参数asynLoadingSegment 设为true.
在第一个buffer使用完毕之后，切换到另一个buffer，需要去验证该buffer是否加载完成数据，然后进行切换（对于异步加载出了异常则同步加载数据，然后再切换，此时会产生发号的阻塞）。

##使用示例

    <bean id="idLeafService" class="com.zhuzhong.idleaf.support.MysqlIdLeafServiceImpl">
		<property name="jdbcTemplate" ref="jdbcTemplate" />
	</bean>

    Long id=idLeafService.getId();