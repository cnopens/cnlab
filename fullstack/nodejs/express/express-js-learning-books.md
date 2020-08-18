#express-js-learning-books.md

---
MVC Frameworks

MVC (Model View Controller) architecture is a useful design pattern that splits application logic into three parts — models to define the shape of the data, views to organize the user interface, and controllers to communicate between the two. Node frameworks that support MVC architecture are useful if you don’t want to spend too much time worrying about how to organize your code.


## WWH?

What?
Express touts itself as a “fast, un-opinionated, minimalist framework for Node.” The focus is on performance and providing only what you need. The framework provides little functionality of its own, instead of allowing for extensive middleware chaining to process requests.

Why?

我们不用看什么就看star就ok l 
We may as well get Express out of the way first, as no list of Node frameworks would be complete without it. Express is still the reigning champion of popular frameworks, as its 47.5K Github stars, 1.8K watches, and 7.7K forks will attest. This alone makes it an excellent choice. It is robust, well-tested, and there is a large community maintaining and working with it who can support you and answer questions.

How ?





## 常见问题

1. 请问express的req.body.userName为什么点不出来东西，req.body确实有这个属性？

```
	router.posts('/login',function(req,res,next){
		var param = req.body


		
		var a=req.body.userName
		console.log(req.body)
		console.log(a)
	})

```




