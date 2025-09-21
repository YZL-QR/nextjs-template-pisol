本项目核心技术栈：
nextjs+shadnc-ui
supabase
CI/CD部署：vercel

整体的prd文档包括:
- 产品功能文档
./doc/prd.md
从产品经理视角极简的功能，页面描述，面向程序员的brief
要包括：用户使用操作路径设计
不需要：产品价值、商业价值、扩展设计

- 技术API文档
./doc/api.md
实现业务功能的必须api支持（仅需要api定义头，定义输入，输出，不要实现明细）
需要引用的外部框架
不含：数据服务明细（参见supabase.md）、安全策略设置
禁止：在api.md中详细列出每个api的实现细节

- 数据库表文档
./doc/supabase.md
只包含：
project_url
annokey
schema
业务表的ddl、示例数据
不需要：索引说明，安全策略、数据操作函数、CRUD相关代码、扩展设计、性能优化设计

以上doc中，不要使用加粗格式