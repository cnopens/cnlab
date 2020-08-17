#go-gin-http-收发请求.md
---

   formData := make(map[string]interface{})
   // 调用json包的解析，解析请求body
   json.NewDecoder(r.Body).Decode(&formData)
   for key,value := range formData{
      log.Println("key:",key," => value :",value)
   }


   注意： r.Body <==>gin :
   json.NewDecoder(r.Body).Decode(&formData)