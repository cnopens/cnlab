#vmware-vsphere-dependence-install.md


## maven-checkstyle-plugin 使用





## local maven install
mvn install:install-file -Dfile=/media/cnopens/DevWspace/trainingCenter/cloudSpace/vsphere-samples-nodeps/lib/oidc-oauth2-sdk-0.0.1.jar -DgroupId=com.vmware.vapi -DartifactId=oidc-oauth2-sdk -Dversion=0.0.1 -Dpackaging=jar

mvn install:install-file -Dfile=/media/cnopens/DevWspace/trainingCenter/cloudSpace/vsphere-samples-nodeps/lib/vmc-draas-bindings-1.20.0.jar -DgroupId=com.vmware.vmc -DartifactId=vmc-draas-bindings -Dversion=1.20.0 -Dpackaging=jar





<vapi.version>2.19.0</vapi.version>
<slf4j.version>1.7.12</slf4j.version>
<httpclient.version>4.5.10</httpclient.version>
<httpasyncclient.version>4.1.4</httpasyncclient.version>
<jackson2.version>2.10.0</jackson2.version>
<log4j.version>1.2.17</log4j.version>
<commons-lang3.version>3.9</commons-lang3.version>
<commons-configuration.version>1.10</commons-configuration.version>
<commons-cli.version>1.4</commons-cli.version>
<commons-beanutils.version>1.9.4</commons-beanutils.version>
<lookupservice.version>1.0.0</lookupservice.version>
<vspherewssdk.version>6.7.3</vspherewssdk.version>
<vsphereautomationsdk.version>3.5.0</vsphereautomationsdk.version>
<vmc.version>1.33.0</vmc.version>
<nsxpolicysdk.version>3.0.2.0.0.16837625</nsxpolicysdk.version>
<nsxvmcawsint.version>3.0.2.0.0.16837625</nsxvmcawsint.version>
<nsxvmcsdkcommon.version>3.0.2.0.0.16837625</nsxvmcsdkcommon.version>
<vmc.draas.version>1.20.0</vmc.draas.version>
<oidc-oauth2.version>0.0.1</oidc-oauth2.version>


  <dependency>
   <groupId>com.vmware.vmc</groupId>
   <artifactId>vmc-draas-bindings</artifactId>
   <version>${vmc.draas.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.vapi</groupId>
    <artifactId>vapi-runtime</artifactId>
    <version>${vapi.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.vapi</groupId>
    <artifactId>vapi-authentication</artifactId>
    <version>${vapi.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.vapi</groupId>
    <artifactId>vapi-samltoken</artifactId>
    <version>${vapi.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware</groupId>
    <artifactId>vsphereautomation-client-sdk</artifactId>
    <version>${vsphereautomationsdk.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.vmc</groupId>
    <artifactId>vmc-bindings</artifactId>
    <version>${vmc.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.vapi</groupId>
    <artifactId>vapi-vmc-sdk</artifactId>
    <version>${vapi.version}</version>
  </dependency>
  <dependency>
   <groupId>com.vmware.nsx</groupId>
   <artifactId>nsx-vmc-policy-java-sdk</artifactId>
   <version>${nsxpolicysdk.version}</version>
 </dependency>
 <dependency>
   <groupId>com.vmware.nsx</groupId>
   <artifactId>nsx-vmc-aws-integration-sdk</artifactId>
   <version>${nsxvmcawsint.version}</version>
 </dependency>
 <dependency>
   <groupId>com.vmware.nsx</groupId>
   <artifactId>nsx-vmc-sdk-common</artifactId>
   <version>${nsxvmcsdkcommon.version}</version>
 </dependency>
  <dependency>
    <groupId>com.vmware.vim25</groupId>
    <artifactId>vim25</artifactId>
    <version>${vspherewssdk.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.sso</groupId>
    <artifactId>ssoclient</artifactId>
    <version>${vspherewssdk.version}</version>
  </dependency>
  <dependency>
    <groupId>com.vmware.vsphereautomation.lookup</groupId>
    <artifactId>vsphereautomation-lookupservice</artifactId>
    <version>${lookupservice.version}</version>
  </dependency>
