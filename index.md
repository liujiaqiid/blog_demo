<h2 name="api_principle">API设计原则及步骤</h2>

基本原则包括如下几点：
  + 以RESTful架构风格组织服务端资源
    + Resource as abstraction
    + Stateless
    + URI as resource identifier
    + Unique method (GET/POST/PUT/DELETE)
    + Compatibility & Extensibility
  + 不绑定前端features
  + 不绑定后端schema
	+ Security(Eg. Login/HIPAA)
	+ Reduce network require times

基本步骤：
  1. 根据[服务端资源组织](#resource_design)来确定请求的路径:Path
  2. 根据资源操作的类型来确定请求方法:Method
  3. 根据请求的具体动作来设置请求参数:Params
  4. 根据请求实体的类型Content-Type确定处理逻辑(默认按application/json处理)
  5. 根据请求的接收类型Accept来判断返回类型(默认返回application/json实体)
