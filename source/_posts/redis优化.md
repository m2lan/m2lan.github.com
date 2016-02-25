title: redis优化
date: 2015-12-10 02:00:33
categories: Redis
tags: Redis
---
## redis优化
* 在使用redis的时候，客户端通过socket向redis服务端发起请求后，等待服务端的返回结果。
* 客户端请求服务器一次就发送一个报文 -> 等待服务端的返回 -> 关闭连接
* 如果是100个请求、1000个请求，那就得请求100次、1000次
* 所以使用多个请求的时候使用管道来操作(`如果管道打包的命令太多占用的内存也会越大，适量`)
* 以下是使用3种方式进行的测试(`循环写入1000次`)：

		public static void main(String[] args) {
		
			long start = System.currentTimeMillis();
			for (int i = 0; i < 1000; i++) {
				template.opsForHash().put("user1", "status" + i, "value" + i);
			}
			System.out.println(System.currentTimeMillis() - start);
			
			start = System.currentTimeMillis();
			final byte[] rawKey = rawKey("user2");
			template.execute(new RedisCallback<Object>() {
	
				@Override
				public Object doInRedis(RedisConnection connection)
						throws DataAccessException {
					for (int i = 0; i < 1000; i++) {
						final byte[] rawHashKey = rawHashKey("status" + i);
						final byte[] rawHashValue = rawHashValue("value" + i);
						
						connection.hSet(rawKey, rawHashKey, rawHashValue);
					}
					return null;
				}
			});
			System.out.println(System.currentTimeMillis() - start);
			
			start = System.currentTimeMillis();
			final byte[] rawKey2 = rawKey("user3");
			template.execute(new RedisCallback<Object>() {
	
				@Override
				public Object doInRedis(RedisConnection connection)
						throws DataAccessException {
					connection.openPipeline();
					for (int i = 0; i < 1000; i++) {
						final byte[] rawHashKey = rawHashKey("status" + i);
						final byte[] rawHashValue = rawHashValue("value" + i);
						
						connection.hSet(rawKey2, rawHashKey, rawHashValue);
					}
					return connection.closePipeline();
				}
			});
			System.out.println(System.currentTimeMillis() - start);
		}
* 请求结果

			20:54:51.653 [main] DEBUG o.s.d.r.core.RedisConnectionUtils - Opening RedisConnection
			20:54:51.693 [main] DEBUG o.s.d.r.core.RedisConnectionUtils - Closing Redis Connection
			42283
			20:54:51.695 [main] DEBUG o.s.d.r.core.RedisConnectionUtils - Opening RedisConnection
			20:55:33.922 [main] DEBUG o.s.d.r.core.RedisConnectionUtils - Closing Redis Connection
			42228
			20:55:33.923 [main] DEBUG o.s.d.r.core.RedisConnectionUtils - Opening RedisConnection
			20:55:34.058 [main] DEBUG o.s.d.r.core.RedisConnectionUtils - Closing Redis Connection
			136
* 可以看出在使用管道打包发送请求所用的时间不到1s。而发送1000次请求所用的时间达到了42s。


