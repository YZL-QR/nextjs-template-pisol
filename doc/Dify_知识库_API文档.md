# 知识库 - Dify

# 知识库 API

### 鉴权

Dify Service API 使用 API-Key 进行鉴权。

建议开发者把 API-Key 放在后端存储，而非分享或者放在客户端存储，以免 API-Key 泄露，导致财产损失。

所有 API 请求都应在 Authorization HTTP Header 中包含您的 API-Key ，如下所示：

### Code

```
  Authorization: Bearer {API_KEY}
```

```
  Authorization: Bearer {API_KEY}
```

## 通过文本创建文档

此接口基于已存在知识库，在此知识库的基础上通过文本创建新的文档

### Path

- Name dataset_id Type string Description 知识库 ID


知识库 ID

### Request Body

- Name name Type string Description 文档名称
- Name text Type string Description 文档内容
- Name doc_type Type string Description 文档类型（选填） book 图书 Book web_page 网页 Web page paper 学术论文/文章 Academic paper/article social_media_post 社交媒体帖子 Social media post wikipedia_entry 维基百科条目 Wikipedia entry personal_document 个人文档 Personal document business_document 商业文档 Business document im_chat_log 即时通讯记录 Chat log synced_from_notion Notion同步文档 Notion document synced_from_github GitHub同步文档 GitHub document others 其他文档类型 Other document types
- Name doc_metadata Type object Description 文档元数据（如提供文档类型则必填）。字段因文档类型而异： 针对图书 For book : title 书名 Book title language 图书语言 Book language author 作者 Book author publisher 出版社 Publisher name publication_date 出版日期 Publication date isbn ISBN号码 ISBN number category 图书分类 Book category 针对网页 For web_page : title 页面标题 Page title url 页面网址 Page URL language 页面语言 Page language publish_date 发布日期 Publish date author/publisher 作者/发布者 Author or publisher topic/keywords 主题/关键词 Topic or keywords description 页面描述 Page description 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象
- Name indexing_technique Type string Description 索引方式 high_quality 高质量：使用  embedding 模型进行嵌入，构建为向量数据库索引 economy 经济：使用 keyword table index 的倒排索引进行构建
- Name doc_form Type string Description 索引内容的形式 text_model text 文档直接 embedding，经济模式默认为该模式 hierarchical_model parent-child 模式 qa_model Q&A 模式：为分片文档生成 Q&A 对，然后对问题进行 embedding
- Name doc_language Type string Description 在 Q&A 模式下，指定文档的语言，例如： English 、 Chinese
- Name process_rule Type object Description 处理规则 mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义 rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）
- 当知识库未设置任何参数的时候，首次上传需要提供以下参数，未提供则使用默认选项：
- Name retrieval_model Type object Description 检索模式 search_method (string) 检索方法 hybrid_search 混合检索 semantic_search 语义检索 full_text_search 全文检索 reranking_enable (bool) 是否开启rerank reranking_model (object) Rerank 模型配置 reranking_provider_name (string) Rerank 模型的提供商 reranking_model_name (string) Rerank 模型的名称 top_k (int) 召回条数 score_threshold_enabled (bool)是否开启召回分数限制 score_threshold (float) 召回分数限制
- Name embedding_model Type string Description Embedding 模型名称
- Name embedding_model_provider Type string Description Embedding 模型供应商


文档名称

文档内容

文档类型（选填）

- book 图书 Book
- web_page 网页 Web page
- paper 学术论文/文章 Academic paper/article
- social_media_post 社交媒体帖子 Social media post
- wikipedia_entry 维基百科条目 Wikipedia entry
- personal_document 个人文档 Personal document
- business_document 商业文档 Business document
- im_chat_log 即时通讯记录 Chat log
- synced_from_notion Notion同步文档 Notion document
- synced_from_github GitHub同步文档 GitHub document
- others 其他文档类型 Other document types


文档元数据（如提供文档类型则必填）。字段因文档类型而异：

针对图书 For book :

- title 书名 Book title
- language 图书语言 Book language
- author 作者 Book author
- publisher 出版社 Publisher name
- publication_date 出版日期 Publication date
- isbn ISBN号码 ISBN number
- category 图书分类 Book category


针对网页 For web_page :

- title 页面标题 Page title
- url 页面网址 Page URL
- language 页面语言 Page language
- publish_date 发布日期 Publish date
- author/publisher 作者/发布者 Author or publisher
- topic/keywords 主题/关键词 Topic or keywords
- description 页面描述 Page description


请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。

针对"其他"类型文档，接受任何有效的JSON对象

索引方式

- high_quality 高质量：使用  embedding 模型进行嵌入，构建为向量数据库索引
- economy 经济：使用 keyword table index 的倒排索引进行构建


索引内容的形式

- text_model text 文档直接 embedding，经济模式默认为该模式
- hierarchical_model parent-child 模式
- qa_model Q&A 模式：为分片文档生成 Q&A 对，然后对问题进行 embedding


在 Q&A 模式下，指定文档的语言，例如： English 、 Chinese

处理规则

- mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义
- rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


- pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值
- segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000
- parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回
- subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


- id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址
- enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值


- 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址


- remove_extra_spaces 替换连续空格、换行符、制表符
- remove_urls_emails 删除 URL、电子邮件地址


- separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n
- max_tokens 最大长度（token）默认为 1000


- separator 分段标识符，目前仅允许设置一个分隔符。默认为 ***
- max_tokens 最大长度 (token) 需要校验小于父级的长度
- chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


检索模式

- search_method (string) 检索方法 hybrid_search 混合检索 semantic_search 语义检索 full_text_search 全文检索
- reranking_enable (bool) 是否开启rerank
- reranking_model (object) Rerank 模型配置 reranking_provider_name (string) Rerank 模型的提供商 reranking_model_name (string) Rerank 模型的名称
- top_k (int) 召回条数
- score_threshold_enabled (bool)是否开启召回分数限制
- score_threshold (float) 召回分数限制


- hybrid_search 混合检索
- semantic_search 语义检索
- full_text_search 全文检索


- reranking_provider_name (string) Rerank 模型的提供商
- reranking_model_name (string) Rerank 模型的名称


Embedding 模型名称

Embedding 模型供应商

### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/document/create-by-text' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{"name": "text","text": "text","indexing_technique": "high_quality","process_rule": {"mode": "automatic"}}'
```

### Response

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
        "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "text.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695690280,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": ""
}
```

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
        "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "text.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695690280,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": ""
}
```

## 通过文件创建文档

此接口基于已存在知识库，在此知识库的基础上通过文件创建新的文档

### Path

- Name dataset_id Type string Description 知识库 ID


知识库 ID

### Request Body

- Name data Type multipart/form-data json string Description original_document_id 源文档 ID（选填） 用于重新上传文档或修改文档清洗、分段配置，缺失的信息从源文档复制 源文档不可为归档的文档 当传入 original_document_id 时，代表文档进行更新操作， process_rule 为可填项目，不填默认使用源文档的分段方式 未传入 original_document_id 时，代表文档进行新增操作， process_rule 为必填 indexing_technique 索引方式 high_quality 高质量：使用  embedding 模型进行嵌入，构建为向量数据库索引 economy 经济：使用 keyword table index 的倒排索引进行构建 doc_form 索引内容的形式 text_model text 文档直接 embedding，经济模式默认为该模式 hierarchical_model parent-child 模式 qa_model Q&A 模式：为分片文档生成 Q&A 对，然后对问题进行 embedding doc_type 文档类型（选填）Type of document (optional) book 图书
文档记录一本书籍或出版物 web_page 网页
网页内容的文档记录 paper 学术论文/文章
学术论文或研究文章的记录 social_media_post 社交媒体帖子
社交媒体上的帖子内容 wikipedia_entry 维基百科条目
维基百科的词条内容 personal_document 个人文档
个人相关的文档记录 business_document 商业文档
商业相关的文档记录 im_chat_log 即时通讯记录
即时通讯的聊天记录 synced_from_notion Notion同步文档
从Notion同步的文档内容 synced_from_github GitHub同步文档
从GitHub同步的文档内容 others 其他文档类型
其他未列出的文档类型 doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异 针对图书类型 For book : title 书名
书籍的标题 language 图书语言
书籍的语言 author 作者
书籍的作者 publisher 出版社
出版社的名称 publication_date 出版日期
书籍的出版日期 isbn ISBN号码
书籍的ISBN编号 category 图书分类
书籍的分类类别 针对网页类型 For web_page : title 页面标题
网页的标题 url 页面网址
网页的URL地址 language 页面语言
网页的语言 publish_date 发布日期
网页的发布日期 author/publisher 作者/发布者
网页的作者或发布者 topic/keywords 主题/关键词
网页的主题或关键词 description 页面描述
网页的描述信息 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象 doc_language 在 Q&A 模式下，指定文档的语言，例如： English 、 Chinese process_rule 处理规则 mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义 rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）
- Name file Type multipart/form-data Description 需要上传的文件。
- 当知识库未设置任何参数的时候，首次上传需要提供以下参数，未提供则使用默认选项：
- Name retrieval_model Type object Description 检索模式 search_method (string) 检索方法 hybrid_search 混合检索 semantic_search 语义检索 full_text_search 全文检索 reranking_enable (bool) 是否开启rerank reranking_model (object) Rerank 模型配置 reranking_provider_name (string) Rerank 模型的提供商 reranking_model_name (string) Rerank 模型的名称 top_k (int) 召回条数 score_threshold_enabled (bool)是否开启召回分数限制 score_threshold (float) 召回分数限制
- Name embedding_model Type string Description Embedding 模型名称
- Name embedding_model_provider Type string Description Embedding 模型供应商


- original_document_id 源文档 ID（选填） 用于重新上传文档或修改文档清洗、分段配置，缺失的信息从源文档复制 源文档不可为归档的文档 当传入 original_document_id 时，代表文档进行更新操作， process_rule 为可填项目，不填默认使用源文档的分段方式 未传入 original_document_id 时，代表文档进行新增操作， process_rule 为必填
- indexing_technique 索引方式 high_quality 高质量：使用  embedding 模型进行嵌入，构建为向量数据库索引 economy 经济：使用 keyword table index 的倒排索引进行构建
- doc_form 索引内容的形式 text_model text 文档直接 embedding，经济模式默认为该模式 hierarchical_model parent-child 模式 qa_model Q&A 模式：为分片文档生成 Q&A 对，然后对问题进行 embedding
- doc_type 文档类型（选填）Type of document (optional) book 图书
文档记录一本书籍或出版物 web_page 网页
网页内容的文档记录 paper 学术论文/文章
学术论文或研究文章的记录 social_media_post 社交媒体帖子
社交媒体上的帖子内容 wikipedia_entry 维基百科条目
维基百科的词条内容 personal_document 个人文档
个人相关的文档记录 business_document 商业文档
商业相关的文档记录 im_chat_log 即时通讯记录
即时通讯的聊天记录 synced_from_notion Notion同步文档
从Notion同步的文档内容 synced_from_github GitHub同步文档
从GitHub同步的文档内容 others 其他文档类型
其他未列出的文档类型
- doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异 针对图书类型 For book : title 书名
书籍的标题 language 图书语言
书籍的语言 author 作者
书籍的作者 publisher 出版社
出版社的名称 publication_date 出版日期
书籍的出版日期 isbn ISBN号码
书籍的ISBN编号 category 图书分类
书籍的分类类别 针对网页类型 For web_page : title 页面标题
网页的标题 url 页面网址
网页的URL地址 language 页面语言
网页的语言 publish_date 发布日期
网页的发布日期 author/publisher 作者/发布者
网页的作者或发布者 topic/keywords 主题/关键词
网页的主题或关键词 description 页面描述
网页的描述信息 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象
- doc_language 在 Q&A 模式下，指定文档的语言，例如： English 、 Chinese
- process_rule 处理规则 mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义 rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


original_document_id 源文档 ID（选填）

- 用于重新上传文档或修改文档清洗、分段配置，缺失的信息从源文档复制
- 源文档不可为归档的文档
- 当传入 original_document_id 时，代表文档进行更新操作， process_rule 为可填项目，不填默认使用源文档的分段方式
- 未传入 original_document_id 时，代表文档进行新增操作， process_rule 为必填


indexing_technique 索引方式

- high_quality 高质量：使用  embedding 模型进行嵌入，构建为向量数据库索引
- economy 经济：使用 keyword table index 的倒排索引进行构建


doc_form 索引内容的形式

- text_model text 文档直接 embedding，经济模式默认为该模式
- hierarchical_model parent-child 模式
- qa_model Q&A 模式：为分片文档生成 Q&A 对，然后对问题进行 embedding


doc_type 文档类型（选填）Type of document (optional)

- book 图书
文档记录一本书籍或出版物
- web_page 网页
网页内容的文档记录
- paper 学术论文/文章
学术论文或研究文章的记录
- social_media_post 社交媒体帖子
社交媒体上的帖子内容
- wikipedia_entry 维基百科条目
维基百科的词条内容
- personal_document 个人文档
个人相关的文档记录
- business_document 商业文档
商业相关的文档记录
- im_chat_log 即时通讯记录
即时通讯的聊天记录
- synced_from_notion Notion同步文档
从Notion同步的文档内容
- synced_from_github GitHub同步文档
从GitHub同步的文档内容
- others 其他文档类型
其他未列出的文档类型


doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异

针对图书类型 For book :

- title 书名
书籍的标题
- language 图书语言
书籍的语言
- author 作者
书籍的作者
- publisher 出版社
出版社的名称
- publication_date 出版日期
书籍的出版日期
- isbn ISBN号码
书籍的ISBN编号
- category 图书分类
书籍的分类类别


针对网页类型 For web_page :

- title 页面标题
网页的标题
- url 页面网址
网页的URL地址
- language 页面语言
网页的语言
- publish_date 发布日期
网页的发布日期
- author/publisher 作者/发布者
网页的作者或发布者
- topic/keywords 主题/关键词
网页的主题或关键词
- description 页面描述
网页的描述信息


请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。

针对"其他"类型文档，接受任何有效的JSON对象

doc_language 在 Q&A 模式下，指定文档的语言，例如： English 、 Chinese

process_rule 处理规则

- mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义
- rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


- pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值
- segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000
- parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回
- subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


- id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址
- enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值


- 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址


- remove_extra_spaces 替换连续空格、换行符、制表符
- remove_urls_emails 删除 URL、电子邮件地址


- separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n
- max_tokens 最大长度（token）默认为 1000


- separator 分段标识符，目前仅允许设置一个分隔符。默认为 ***
- max_tokens 最大长度 (token) 需要校验小于父级的长度
- chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


需要上传的文件。

检索模式

- search_method (string) 检索方法 hybrid_search 混合检索 semantic_search 语义检索 full_text_search 全文检索
- reranking_enable (bool) 是否开启rerank
- reranking_model (object) Rerank 模型配置 reranking_provider_name (string) Rerank 模型的提供商 reranking_model_name (string) Rerank 模型的名称
- top_k (int) 召回条数
- score_threshold_enabled (bool)是否开启召回分数限制
- score_threshold (float) 召回分数限制


- hybrid_search 混合检索
- semantic_search 语义检索
- full_text_search 全文检索


- reranking_provider_name (string) Rerank 模型的提供商
- reranking_model_name (string) Rerank 模型的名称


Embedding 模型名称

Embedding 模型供应商

### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/document/create-by-file' \
--header 'Authorization: Bearer {api_key}' \
--form 'data="{"indexing_technique":"high_quality","process_rule":{"rules":{"pre_processing_rules":[{"id":"remove_extra_spaces","enabled":true},{"id":"remove_urls_emails","enabled":true}],"segmentation":{"separator":"###","max_tokens":500}},"mode":"custom"}}";type=text/plain' \
--form 'file=@"/path/to/file"'
```

### Response

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
      "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "Dify.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695308667,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": ""
}
```

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
      "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "Dify.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695308667,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": ""
}
```

## 创建空知识库

### Request Body

- Name name Type string Description 知识库名称（必填）
- Name description Type string Description 知识库描述（选填）
- Name indexing_technique Type string Description 索引模式（选填，建议填写） high_quality 高质量 economy 经济
- Name permission Type string Description 权限（选填，默认 only_me） only_me 仅自己 all_team_members 所有团队成员 partial_members 部分团队成员
- Name provider Type string Description Provider（选填，默认 vendor） vendor 上传文件 external 外部知识库
- Name external_knowledge_api_id Type str Description 外部知识库 API_ID（选填）
- Name external_knowledge_id Type str Description 外部知识库 ID（选填）


知识库名称（必填）

知识库描述（选填）

索引模式（选填，建议填写）

- high_quality 高质量
- economy 经济


权限（选填，默认 only_me）

- only_me 仅自己
- all_team_members 所有团队成员
- partial_members 部分团队成员


Provider（选填，默认 vendor）

- vendor 上传文件
- external 外部知识库


外部知识库 API_ID（选填）

外部知识库 ID（选填）

### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{"name": "name", "permission": "only_me"}'
```

### Response

```
{
  "id": "",
  "name": "name",
  "description": null,
  "provider": "vendor",
  "permission": "only_me",
  "data_source_type": null,
  "indexing_technique": null,
  "app_count": 0,
  "document_count": 0,
  "word_count": 0,
  "created_by": "",
  "created_at": 1695636173,
  "updated_by": "",
  "updated_at": 1695636173,
  "embedding_model": null,
  "embedding_model_provider": null,
  "embedding_available": null
}
```

```
{
  "id": "",
  "name": "name",
  "description": null,
  "provider": "vendor",
  "permission": "only_me",
  "data_source_type": null,
  "indexing_technique": null,
  "app_count": 0,
  "document_count": 0,
  "word_count": 0,
  "created_by": "",
  "created_at": 1695636173,
  "updated_by": "",
  "updated_at": 1695636173,
  "embedding_model": null,
  "embedding_model_provider": null,
  "embedding_available": null
}
```

## 知识库列表

### Query

- Name page Type string Description 页码
- Name limit Type string Description 返回条数，默认 20，范围 1-100


页码

返回条数，默认 20，范围 1-100

### Request

```
curl --location --request GET 'http://ai.pisolutions.cn/v1/datasets?page=1&limit=20' \
--header 'Authorization: Bearer {api_key}'
```

### Response

```
{
  "data": [
    {
      "id": "",
      "name": "知识库名称",
      "description": "描述信息",
      "permission": "only_me",
      "data_source_type": "upload_file",
      "indexing_technique": "",
      "app_count": 2,
      "document_count": 10,
      "word_count": 1200,
      "created_by": "",
      "created_at": "",
      "updated_by": "",
      "updated_at": ""
    },
    ...
  ],
  "has_more": true,
  "limit": 20,
  "total": 50,
  "page": 1
}
```

```
{
  "data": [
    {
      "id": "",
      "name": "知识库名称",
      "description": "描述信息",
      "permission": "only_me",
      "data_source_type": "upload_file",
      "indexing_technique": "",
      "app_count": 2,
      "document_count": 10,
      "word_count": 1200,
      "created_by": "",
      "created_at": "",
      "updated_by": "",
      "updated_at": ""
    },
    ...
  ],
  "has_more": true,
  "limit": 20,
  "total": 50,
  "page": 1
}
```

## 删除知识库

### Path

- Name dataset_id Type string Description 知识库 ID


知识库 ID

### Request

```
curl --location --request DELETE 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}' \
--header 'Authorization: Bearer {api_key}'
```

### Response

```
204 No Content
```

```
204 No Content
```

## 通过文本更新文档

此接口基于已存在知识库，在此知识库的基础上通过文本更新文档

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID


知识库 ID

文档 ID

### Request Body

- Name name Type string Description 文档名称（选填）
- Name text Type string Description 文档内容（选填）
- Name doc_type Type string Description 文档类型（选填） book 图书 Book web_page 网页 Web page paper 学术论文/文章 Academic paper/article social_media_post 社交媒体帖子 Social media post wikipedia_entry 维基百科条目 Wikipedia entry personal_document 个人文档 Personal document business_document 商业文档 Business document im_chat_log 即时通讯记录 Chat log synced_from_notion Notion同步文档 Notion document synced_from_github GitHub同步文档 GitHub document others 其他文档类型 Other document types
- Name doc_metadata Type object Description 文档元数据（如提供文档类型则必填）。字段因文档类型而异： 针对图书 For book : title 书名 Book title language 图书语言 Book language author 作者 Book author publisher 出版社 Publisher name publication_date 出版日期 Publication date isbn ISBN号码 ISBN number category 图书分类 Book category 针对网页 For web_page : title 页面标题 Page title url 页面网址 Page URL language 页面语言 Page language publish_date 发布日期 Publish date author/publisher 作者/发布者 Author or publisher topic/keywords 主题/关键词 Topic or keywords description 页面描述 Page description 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象
- Name process_rule Type object Description 处理规则（选填） mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义 rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


文档名称（选填）

文档内容（选填）

文档类型（选填）

- book 图书 Book
- web_page 网页 Web page
- paper 学术论文/文章 Academic paper/article
- social_media_post 社交媒体帖子 Social media post
- wikipedia_entry 维基百科条目 Wikipedia entry
- personal_document 个人文档 Personal document
- business_document 商业文档 Business document
- im_chat_log 即时通讯记录 Chat log
- synced_from_notion Notion同步文档 Notion document
- synced_from_github GitHub同步文档 GitHub document
- others 其他文档类型 Other document types


文档元数据（如提供文档类型则必填）。字段因文档类型而异：

针对图书 For book :

- title 书名 Book title
- language 图书语言 Book language
- author 作者 Book author
- publisher 出版社 Publisher name
- publication_date 出版日期 Publication date
- isbn ISBN号码 ISBN number
- category 图书分类 Book category


针对网页 For web_page :

- title 页面标题 Page title
- url 页面网址 Page URL
- language 页面语言 Page language
- publish_date 发布日期 Publish date
- author/publisher 作者/发布者 Author or publisher
- topic/keywords 主题/关键词 Topic or keywords
- description 页面描述 Page description


请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。

针对"其他"类型文档，接受任何有效的JSON对象

处理规则（选填）

- mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义
- rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


- pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值
- segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000
- parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回
- subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


- id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址
- enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值


- 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址


- remove_extra_spaces 替换连续空格、换行符、制表符
- remove_urls_emails 删除 URL、电子邮件地址


- separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n
- max_tokens 最大长度（token）默认为 1000


- separator 分段标识符，目前仅允许设置一个分隔符。默认为 ***
- max_tokens 最大长度 (token) 需要校验小于父级的长度
- chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/update-by-text' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{"name": "name","text": "text"}'
```

### Response

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
      "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "name.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695308667,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": ""
}
```

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
      "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "name.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695308667,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": ""
}
```

## 通过文件更新文档

此接口基于已存在知识库，在此知识库的基础上通过文件更新文档的操作。

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID


知识库 ID

文档 ID

### Request Body

- Name name Type string Description 文档名称（选填）
- Name file Type multipart/form-data Description 需要上传的文件
- Name process_rule Type object Description 处理规则（选填） mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义 rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填） doc_type 文档类型（选填）Type of document (optional) book 图书
文档记录一本书籍或出版物 web_page 网页
网页内容的文档记录 paper 学术论文/文章
学术论文或研究文章的记录 social_media_post 社交媒体帖子
社交媒体上的帖子内容 wikipedia_entry 维基百科条目
维基百科的词条内容 personal_document 个人文档
个人相关的文档记录 business_document 商业文档
商业相关的文档记录 im_chat_log 即时通讯记录
即时通讯的聊天记录 synced_from_notion Notion同步文档
从Notion同步的文档内容 synced_from_github GitHub同步文档
从GitHub同步的文档内容 others 其他文档类型
其他未列出的文档类型 doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异 针对图书类型 For book : title 书名
书籍的标题 language 图书语言
书籍的语言 author 作者
书籍的作者 publisher 出版社
出版社的名称 publication_date 出版日期
书籍的出版日期 isbn ISBN号码
书籍的ISBN编号 category 图书分类
书籍的分类类别 针对网页类型 For web_page : title 页面标题
网页的标题 url 页面网址
网页的URL地址 language 页面语言
网页的语言 publish_date 发布日期
网页的发布日期 author/publisher 作者/发布者
网页的作者或发布者 topic/keywords 主题/关键词
网页的主题或关键词 description 页面描述
网页的描述信息 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象


文档名称（选填）

需要上传的文件

处理规则（选填）

- mode (string) 清洗、分段模式 ，automatic 自动 / custom 自定义
- rules (object) 自定义规则（自动模式下，该字段为空） pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值 segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000 parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回 subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填） doc_type 文档类型（选填）Type of document (optional) book 图书
文档记录一本书籍或出版物 web_page 网页
网页内容的文档记录 paper 学术论文/文章
学术论文或研究文章的记录 social_media_post 社交媒体帖子
社交媒体上的帖子内容 wikipedia_entry 维基百科条目
维基百科的词条内容 personal_document 个人文档
个人相关的文档记录 business_document 商业文档
商业相关的文档记录 im_chat_log 即时通讯记录
即时通讯的聊天记录 synced_from_notion Notion同步文档
从Notion同步的文档内容 synced_from_github GitHub同步文档
从GitHub同步的文档内容 others 其他文档类型
其他未列出的文档类型 doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异 针对图书类型 For book : title 书名
书籍的标题 language 图书语言
书籍的语言 author 作者
书籍的作者 publisher 出版社
出版社的名称 publication_date 出版日期
书籍的出版日期 isbn ISBN号码
书籍的ISBN编号 category 图书分类
书籍的分类类别 针对网页类型 For web_page : title 页面标题
网页的标题 url 页面网址
网页的URL地址 language 页面语言
网页的语言 publish_date 发布日期
网页的发布日期 author/publisher 作者/发布者
网页的作者或发布者 topic/keywords 主题/关键词
网页的主题或关键词 description 页面描述
网页的描述信息 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象


- pre_processing_rules (array[object]) 预处理规则 id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址 enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值
- segmentation (object) 分段规则 separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n max_tokens 最大长度（token）默认为 1000
- parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回
- subchunk_segmentation (object) 子分段规则 separator 分段标识符，目前仅允许设置一个分隔符。默认为 *** max_tokens 最大长度 (token) 需要校验小于父级的长度 chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）
- doc_type 文档类型（选填）Type of document (optional) book 图书
文档记录一本书籍或出版物 web_page 网页
网页内容的文档记录 paper 学术论文/文章
学术论文或研究文章的记录 social_media_post 社交媒体帖子
社交媒体上的帖子内容 wikipedia_entry 维基百科条目
维基百科的词条内容 personal_document 个人文档
个人相关的文档记录 business_document 商业文档
商业相关的文档记录 im_chat_log 即时通讯记录
即时通讯的聊天记录 synced_from_notion Notion同步文档
从Notion同步的文档内容 synced_from_github GitHub同步文档
从GitHub同步的文档内容 others 其他文档类型
其他未列出的文档类型
- doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异 针对图书类型 For book : title 书名
书籍的标题 language 图书语言
书籍的语言 author 作者
书籍的作者 publisher 出版社
出版社的名称 publication_date 出版日期
书籍的出版日期 isbn ISBN号码
书籍的ISBN编号 category 图书分类
书籍的分类类别 针对网页类型 For web_page : title 页面标题
网页的标题 url 页面网址
网页的URL地址 language 页面语言
网页的语言 publish_date 发布日期
网页的发布日期 author/publisher 作者/发布者
网页的作者或发布者 topic/keywords 主题/关键词
网页的主题或关键词 description 页面描述
网页的描述信息 请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。 针对"其他"类型文档，接受任何有效的JSON对象


pre_processing_rules (array[object]) 预处理规则

- id (string) 预处理规则的唯一标识符 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址
- enabled (bool) 是否选中该规则，不传入文档 ID 时代表默认值


- 枚举： remove_extra_spaces 替换连续空格、换行符、制表符 remove_urls_emails 删除 URL、电子邮件地址


- remove_extra_spaces 替换连续空格、换行符、制表符
- remove_urls_emails 删除 URL、电子邮件地址


segmentation (object) 分段规则

- separator 自定义分段标识符，目前仅允许设置一个分隔符。默认为 \n
- max_tokens 最大长度（token）默认为 1000


parent_mode 父分段的召回模式 full-doc 全文召回 / paragraph 段落召回

subchunk_segmentation (object) 子分段规则

- separator 分段标识符，目前仅允许设置一个分隔符。默认为 ***
- max_tokens 最大长度 (token) 需要校验小于父级的长度
- chunk_overlap 分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分（选填）


doc_type 文档类型（选填）Type of document (optional)

- book 图书
文档记录一本书籍或出版物
- web_page 网页
网页内容的文档记录
- paper 学术论文/文章
学术论文或研究文章的记录
- social_media_post 社交媒体帖子
社交媒体上的帖子内容
- wikipedia_entry 维基百科条目
维基百科的词条内容
- personal_document 个人文档
个人相关的文档记录
- business_document 商业文档
商业相关的文档记录
- im_chat_log 即时通讯记录
即时通讯的聊天记录
- synced_from_notion Notion同步文档
从Notion同步的文档内容
- synced_from_github GitHub同步文档
从GitHub同步的文档内容
- others 其他文档类型
其他未列出的文档类型


doc_metadata 文档元数据（如提供文档类型则必填
字段因文档类型而异

针对图书类型 For book :

- title 书名
书籍的标题
- language 图书语言
书籍的语言
- author 作者
书籍的作者
- publisher 出版社
出版社的名称
- publication_date 出版日期
书籍的出版日期
- isbn ISBN号码
书籍的ISBN编号
- category 图书分类
书籍的分类类别


针对网页类型 For web_page :

- title 页面标题
网页的标题
- url 页面网址
网页的URL地址
- language 页面语言
网页的语言
- publish_date 发布日期
网页的发布日期
- author/publisher 作者/发布者
网页的作者或发布者
- topic/keywords 主题/关键词
网页的主题或关键词
- description 页面描述
网页的描述信息


请查看 api/services/dataset_service.py 了解各文档类型所需字段的详细信息。

针对"其他"类型文档，接受任何有效的JSON对象

### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/update-by-file' \
--header 'Authorization: Bearer {api_key}' \
--form 'data="{"name":"Dify","indexing_technique":"high_quality","process_rule":{"rules":{"pre_processing_rules":[{"id":"remove_extra_spaces","enabled":true},{"id":"remove_urls_emails","enabled":true}],"segmentation":{"separator":"###","max_tokens":500}},"mode":"custom"}}";type=text/plain' \
--form 'file=@"/path/to/file"'
```

### Response

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
      "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "Dify.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695308667,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": "20230921150427533684"
}
```

```
{
  "document": {
    "id": "",
    "position": 1,
    "data_source_type": "upload_file",
    "data_source_info": {
      "upload_file_id": ""
    },
    "dataset_process_rule_id": "",
    "name": "Dify.txt",
    "created_from": "api",
    "created_by": "",
    "created_at": 1695308667,
    "tokens": 0,
    "indexing_status": "waiting",
    "error": null,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "archived": false,
    "display_status": "queuing",
    "word_count": 0,
    "hit_count": 0,
    "doc_form": "text_model"
  },
  "batch": "20230921150427533684"
}
```

## 获取文档嵌入状态（进度）

### Path

- Name dataset_id Type string Description 知识库 ID
- Name batch Type string Description 上传文档的批次号


知识库 ID

上传文档的批次号

### Request

```
curl --location --request GET 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{batch}/indexing-status' \
--header 'Authorization: Bearer {api_key}'
```

### Response

```
{
  "data":[{
    "id": "",
    "indexing_status": "indexing",
    "processing_started_at": 1681623462.0,
    "parsing_completed_at": 1681623462.0,
    "cleaning_completed_at": 1681623462.0,
    "splitting_completed_at": 1681623462.0,
    "completed_at": null,
    "paused_at": null,
    "error": null,
    "stopped_at": null,
    "completed_segments": 24,
    "total_segments": 100
  }]
}
```

```
{
  "data":[{
    "id": "",
    "indexing_status": "indexing",
    "processing_started_at": 1681623462.0,
    "parsing_completed_at": 1681623462.0,
    "cleaning_completed_at": 1681623462.0,
    "splitting_completed_at": 1681623462.0,
    "completed_at": null,
    "paused_at": null,
    "error": null,
    "stopped_at": null,
    "completed_segments": 24,
    "total_segments": 100
  }]
}
```

## 删除文档

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID


知识库 ID

文档 ID

### Request

```
curl --location --request DELETE 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}' \
--header 'Authorization: Bearer {api_key}'
```

### Response

```
{
  "result": "success"
}
```

```
{
  "result": "success"
}
```

## 知识库文档列表

### Path

- Name dataset_id Type string Description 知识库 ID


知识库 ID

### Query

- Name keyword Type string Description 搜索关键词，可选，目前仅搜索文档名称
- Name page Type string Description 页码，可选
- Name limit Type string Description 返回条数，可选，默认 20，范围 1-100


搜索关键词，可选，目前仅搜索文档名称

页码，可选

返回条数，可选，默认 20，范围 1-100

### Request

```
curl --location --request GET 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents' \
--header 'Authorization: Bearer {api_key}'
```

### Response

```
{
  "data": [
    {
      "id": "",
      "position": 1,
      "data_source_type": "file_upload",
      "data_source_info": null,
      "dataset_process_rule_id": null,
      "name": "dify",
      "created_from": "",
      "created_by": "",
      "created_at": 1681623639,
      "tokens": 0,
      "indexing_status": "waiting",
      "error": null,
      "enabled": true,
      "disabled_at": null,
      "disabled_by": null,
      "archived": false
    },
  ],
  "has_more": false,
  "limit": 20,
  "total": 9,
  "page": 1
}
```

```
{
  "data": [
    {
      "id": "",
      "position": 1,
      "data_source_type": "file_upload",
      "data_source_info": null,
      "dataset_process_rule_id": null,
      "name": "dify",
      "created_from": "",
      "created_by": "",
      "created_at": 1681623639,
      "tokens": 0,
      "indexing_status": "waiting",
      "error": null,
      "enabled": true,
      "disabled_at": null,
      "disabled_by": null,
      "archived": false
    },
  ],
  "has_more": false,
  "limit": 20,
  "total": 9,
  "page": 1
}
```

## 新增分段

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID


知识库 ID

文档 ID

### Request Body

- Name segments Type object list Description content (text) 文本内容/问题内容，必填 answer (text) 答案内容，非必填，如果知识库的模式为 Q&A 模式则传值 keywords (list) 关键字，非必填


- content (text) 文本内容/问题内容，必填
- answer (text) 答案内容，非必填，如果知识库的模式为 Q&A 模式则传值
- keywords (list) 关键字，非必填


### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/segments' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{"segments": [{"content": "1","answer": "1","keywords": ["a"]}]}'
```

### Response

```
{
  "data": [{
    "id": "",
    "position": 1,
    "document_id": "",
    "content": "1",
    "answer": "1",
    "word_count": 25,
    "tokens": 0,
    "keywords": [
        "a"
    ],
    "index_node_id": "",
    "index_node_hash": "",
    "hit_count": 0,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "status": "completed",
    "created_by": "",
    "created_at": 1695312007,
    "indexing_at": 1695312007,
    "completed_at": 1695312007,
    "error": null,
    "stopped_at": null
  }],
  "doc_form": "text_model"
}
```

```
{
  "data": [{
    "id": "",
    "position": 1,
    "document_id": "",
    "content": "1",
    "answer": "1",
    "word_count": 25,
    "tokens": 0,
    "keywords": [
        "a"
    ],
    "index_node_id": "",
    "index_node_hash": "",
    "hit_count": 0,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "status": "completed",
    "created_by": "",
    "created_at": 1695312007,
    "indexing_at": 1695312007,
    "completed_at": 1695312007,
    "error": null,
    "stopped_at": null
  }],
  "doc_form": "text_model"
}
```

## 查询文档分段

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID


知识库 ID

文档 ID

### Query

- Name keyword Type string Description 搜索关键词，可选
- Name status Type string Description 搜索状态，completed


搜索关键词，可选

搜索状态，completed

### Request

```
curl --location --request GET 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/segments' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json'
```

### Response

```
{
  "data": [{
    "id": "",
    "position": 1,
    "document_id": "",
    "content": "1",
    "answer": "1",
    "word_count": 25,
    "tokens": 0,
    "keywords": [
        "a"
    ],
    "index_node_id": "",
    "index_node_hash": "",
    "hit_count": 0,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "status": "completed",
    "created_by": "",
    "created_at": 1695312007,
    "indexing_at": 1695312007,
    "completed_at": 1695312007,
    "error": null,
    "stopped_at": null
  }],
  "doc_form": "text_model"
}
```

```
{
  "data": [{
    "id": "",
    "position": 1,
    "document_id": "",
    "content": "1",
    "answer": "1",
    "word_count": 25,
    "tokens": 0,
    "keywords": [
        "a"
    ],
    "index_node_id": "",
    "index_node_hash": "",
    "hit_count": 0,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "status": "completed",
    "created_by": "",
    "created_at": 1695312007,
    "indexing_at": 1695312007,
    "completed_at": 1695312007,
    "error": null,
    "stopped_at": null
  }],
  "doc_form": "text_model"
}
```

## 删除文档分段

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID
- Name segment_id Type string Description 文档分段ID


知识库 ID

文档 ID

文档分段ID

### Request

```
curl --location --request DELETE 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/segments/{segment_id}' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json'
```

### Response

```
{
  "result": "success"
}
```

```
{
  "result": "success"
}
```

## 更新文档分段

### POST

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID
- Name segment_id Type string Description 文档分段ID


知识库 ID

文档 ID

文档分段ID

### Request Body

- Name segment Type object Description content (text) 文本内容/问题内容，必填 answer (text) 答案内容，非必填，如果知识库的模式为 Q&A 模式则传值 keywords (list) 关键字，非必填 enabled (bool) false/true，非必填 regenerate_child_chunks (bool) 是否重新生成子分段，非必填


- content (text) 文本内容/问题内容，必填
- answer (text) 答案内容，非必填，如果知识库的模式为 Q&A 模式则传值
- keywords (list) 关键字，非必填
- enabled (bool) false/true，非必填
- regenerate_child_chunks (bool) 是否重新生成子分段，非必填


### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/segments/{segment_id}' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json'\
--data-raw '{"segment": {"content": "1","answer": "1", "keywords": ["a"], "enabled": false}}'
```

### Response

```
{
  "data": [{
    "id": "",
    "position": 1,
    "document_id": "",
    "content": "1",
    "answer": "1",
    "word_count": 25,
    "tokens": 0,
    "keywords": [
        "a"
    ],
    "index_node_id": "",
    "index_node_hash": "",
    "hit_count": 0,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "status": "completed",
    "created_by": "",
    "created_at": 1695312007,
    "indexing_at": 1695312007,
    "completed_at": 1695312007,
    "error": null,
    "stopped_at": null
  }],
  "doc_form": "text_model"
}
```

```
{
  "data": [{
    "id": "",
    "position": 1,
    "document_id": "",
    "content": "1",
    "answer": "1",
    "word_count": 25,
    "tokens": 0,
    "keywords": [
        "a"
    ],
    "index_node_id": "",
    "index_node_hash": "",
    "hit_count": 0,
    "enabled": true,
    "disabled_at": null,
    "disabled_by": null,
    "status": "completed",
    "created_by": "",
    "created_at": 1695312007,
    "indexing_at": 1695312007,
    "completed_at": 1695312007,
    "error": null,
    "stopped_at": null
  }],
  "doc_form": "text_model"
}
```

## 获取上传文件

### Path

- Name dataset_id Type string Description 知识库 ID
- Name document_id Type string Description 文档 ID


知识库 ID

文档 ID

### Request

```
curl --location --request GET 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/documents/{document_id}/upload-file' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json'
```

### Response

```
{
  "id": "file_id",
  "name": "file_name",
  "size": 1024,
  "extension": "txt",
  "url": "preview_url",
  "download_url": "download_url",
  "mime_type": "text/plain",
  "created_by": "user_id",
  "created_at": 1728734540,
}
```

```
{
  "id": "file_id",
  "name": "file_name",
  "size": 1024,
  "extension": "txt",
  "url": "preview_url",
  "download_url": "download_url",
  "mime_type": "text/plain",
  "created_by": "user_id",
  "created_at": 1728734540,
}
```

## 检索知识库

### Path

- Name dataset_id Type string Description 知识库 ID


知识库 ID

### Request Body

- Name query Type string Description 检索关键词
- Name retrieval_model Type object Description 检索参数（选填，如不填，按照默认方式召回） search_method (text) 检索方法：以下三个关键字之一，必填 keyword_search 关键字检索 semantic_search 语义检索 full_text_search 全文检索 hybrid_search 混合检索 reranking_enable (bool) 是否启用 Reranking，非必填，如果检索模式为 semantic_search 模式或者 hybrid_search 则传值 reranking_mode (object) Rerank 模型配置，非必填，如果启用了 reranking 则传值 reranking_provider_name (string) Rerank 模型提供商 reranking_model_name (string) Rerank 模型名称 weights (float) 混合检索模式下语意检索的权重设置 top_k (integer) 返回结果数量，非必填 score_threshold_enabled (bool) 是否开启 score 阈值 score_threshold (float) Score 阈值
- Name external_retrieval_model Type object Description 未启用字段


检索关键词

检索参数（选填，如不填，按照默认方式召回）

- search_method (text) 检索方法：以下三个关键字之一，必填 keyword_search 关键字检索 semantic_search 语义检索 full_text_search 全文检索 hybrid_search 混合检索
- reranking_enable (bool) 是否启用 Reranking，非必填，如果检索模式为 semantic_search 模式或者 hybrid_search 则传值
- reranking_mode (object) Rerank 模型配置，非必填，如果启用了 reranking 则传值 reranking_provider_name (string) Rerank 模型提供商 reranking_model_name (string) Rerank 模型名称
- weights (float) 混合检索模式下语意检索的权重设置
- top_k (integer) 返回结果数量，非必填
- score_threshold_enabled (bool) 是否开启 score 阈值
- score_threshold (float) Score 阈值


- keyword_search 关键字检索
- semantic_search 语义检索
- full_text_search 全文检索
- hybrid_search 混合检索


- reranking_provider_name (string) Rerank 模型提供商
- reranking_model_name (string) Rerank 模型名称


未启用字段

### Request

```
curl --location --request POST 'http://ai.pisolutions.cn/v1/datasets/{dataset_id}/retrieve' \
--header 'Authorization: Bearer {api_key}'\
--header 'Content-Type: application/json'\
--data-raw '{
    "query": "test",
    "retrieval_model": {
        "search_method": "keyword_search",
        "reranking_enable": false,
        "reranking_mode": null,
        "reranking_model": {
            "reranking_provider_name": "",
            "reranking_model_name": ""
        },
        "weights": null,
        "top_k": 1,
        "score_threshold_enabled": false,
        "score_threshold": null
    }
}'
```

### Response

```
{
  "query": {
    "content": "test"
  },
  "records": [
    {
      "segment": {
        "id": "7fa6f24f-8679-48b3-bc9d-bdf28d73f218",
        "position": 1,
        "document_id": "a8c6c36f-9f5d-4d7a-8472-f5d7b75d71d2",
        "content": "Operation guide",
        "answer": null,
        "word_count": 847,
        "tokens": 280,
        "keywords": [
          "install",
          "java",
          "base",
          "scripts",
          "jdk",
          "manual",
          "internal",
          "opens",
          "add",
          "vmoptions"
        ],
        "index_node_id": "39dd8443-d960-45a8-bb46-7275ad7fbc8e",
        "index_node_hash": "0189157697b3c6a418ccf8264a09699f25858975578f3467c76d6bfc94df1d73",
        "hit_count": 0,
        "enabled": true,
        "disabled_at": null,
        "disabled_by": null,
        "status": "completed",
        "created_by": "dbcb1ab5-90c8-41a7-8b78-73b235eb6f6f",
        "created_at": 1728734540,
        "indexing_at": 1728734552,
        "completed_at": 1728734584,
        "error": null,
        "stopped_at": null,
        "document": {
          "id": "a8c6c36f-9f5d-4d7a-8472-f5d7b75d71d2",
          "data_source_type": "upload_file",
          "name": "readme.txt",
          "doc_type": null
        }
      },
      "score": 3.730463140527718e-05,
      "tsne_position": null
    }
  ]
}
```

```
{
  "query": {
    "content": "test"
  },
  "records": [
    {
      "segment": {
        "id": "7fa6f24f-8679-48b3-bc9d-bdf28d73f218",
        "position": 1,
        "document_id": "a8c6c36f-9f5d-4d7a-8472-f5d7b75d71d2",
        "content": "Operation guide",
        "answer": null,
        "word_count": 847,
        "tokens": 280,
        "keywords": [
          "install",
          "java",
          "base",
          "scripts",
          "jdk",
          "manual",
          "internal",
          "opens",
          "add",
          "vmoptions"
        ],
        "index_node_id": "39dd8443-d960-45a8-bb46-7275ad7fbc8e",
        "index_node_hash": "0189157697b3c6a418ccf8264a09699f25858975578f3467c76d6bfc94df1d73",
        "hit_count": 0,
        "enabled": true,
        "disabled_at": null,
        "disabled_by": null,
        "status": "completed",
        "created_by": "dbcb1ab5-90c8-41a7-8b78-73b235eb6f6f",
        "created_at": 1728734540,
        "indexing_at": 1728734552,
        "completed_at": 1728734584,
        "error": null,
        "stopped_at": null,
        "document": {
          "id": "a8c6c36f-9f5d-4d7a-8472-f5d7b75d71d2",
          "data_source_type": "upload_file",
          "name": "readme.txt",
          "doc_type": null
        }
      },
      "score": 3.730463140527718e-05,
      "tsne_position": null
    }
  ]
}
```

### 错误信息

- Name code Type string Description 返回的错误代码


返回的错误代码

- Name status Type number Description 返回的错误状态


返回的错误状态

- Name message Type string Description 返回的错误信息


返回的错误信息

### Example

```
  {
    "code": "no_file_uploaded",
    "message": "Please upload your file.",
    "status": 400
  }
```

```
  {
    "code": "no_file_uploaded",
    "message": "Please upload your file.",
    "status": 400
  }
```

|code|status|message|
|---|---|---|
|no_file_uploaded|400|Please upload your file.|
|too_many_files|400|Only one file is allowed.|
|file_too_large|413|File size exceeded.|
|unsupported_file_type|415|File type not allowed.|
|high_quality_dataset_only|400|Current operation only supports 'high-quality' datasets.|
|dataset_not_initialized|400|The dataset is still being initialized or indexing. Please wait a moment.|
|archived_document_immutable|403|The archived document is not editable.|
|dataset_name_duplicate|409|The dataset name already exists. Please modify your dataset name.|
|invalid_action|400|Invalid action.|
|document_already_finished|400|The document has been processed. Please refresh the page or go to the document details.|
|document_indexing|400|The document is being processed and cannot be edited.|
|invalid_metadata|400|The metadata content is incorrect. Please check and verify.|

