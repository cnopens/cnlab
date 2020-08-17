#express-js-learning-books.md



## 常见问题

请问express的req.body.userName为什么点不出来东西，req.body确实有这个属性？

router.posts('/login',function(req,res,next){
	var param = req.body
	var a=req.body.userName
	console.log(req.body)
	console.log(a)
})