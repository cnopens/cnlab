# spring-trans.md

---
## Releasing transactional SqlSession ?


##定时任务
//    @Scheduled(initialDelay = 10000L, fixedDelay = 5000L)
    public void scheduledTask2() {
        System.out.println("任务2执行时间：" + System.currentTimeMillis());
        System.out.println("定时任务2");
        try {
            Thread.sleep(2000L);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("任务2结束时间：" + System.currentTimeMillis());
    }




    