#jdk-relation-qa.md

-----------------------------------------------------------------
sun.misc.BASE64Encoder Error class not exits?


方法一：其中之一的解决方法为：将jdk从12换回8即可

从 java 8 开始，就用 java.util.Base64 工具类来替换 sun.misc.BASE64Encoder 了

或者：

方法二：

mport sun.misc.BASE64Encoder;

import sun.misc.BASE64Decoder;

在项目中，设计到64位编码的。有时开发会用到JDK中自带的BASE64工具。但sun公司是建议不这样做的。尤其是更新了JDK版本，项目甚至还存在保存的信息。可引用 import org.apache.commons.codec.binary.Base64;进行替换

一种解决方案：

原来使用的JDK自带jar包中的
return new BASE64Encoder().encode(encrypted);
替换为
import org.apache.commons.codec.binary.Base64;
return Base64.encodeBase64String(encrypted);

将byte[] encrypted1 = new BASE64Decoder().decodeBuffer(text);
替换为
import org.apache.commons.codec.binary.Base64;
byte[] encrypted1 =Base64.decodeBase64(tex



