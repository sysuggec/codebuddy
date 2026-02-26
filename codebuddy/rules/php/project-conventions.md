# PHP 项目约束与架构约定

> 本文件为项目专属规范，需与 [coding-style.md](coding-style.md) **同时使用**，并遵循 [../common/](../common/) 下的通用规则（如 [security.md](../common/security.md)、[patterns.md](../common/patterns.md) 等）。若与通用规范冲突，以本文件为准。

## 环境与版本

- **Versions**: PHP 7.4, MySQL 5.7；开发环境使用 SQLite 代替（见下方"开发环境 SQLite 差异"）
- **Dependency**: Laravel ORM (Eloquent)，所有 DB 访问必须通过它

## 接口与路由

- **HTTP Methods**: GET + POST（POST 默认 Content-Type 为 `application/x-www-form-urlencoded`，除非另行指定）
- **Entry/Exit**: 统一入口 `action/index.php`
- **Routing**: `?action={模块}.{方法}` → `action/{模块}Control.php` → `{模块}Control::{方法}($request)`
- **Response Format**: JSON 固定结构（**覆盖**通用规则 `patterns.md` 中的 API 响应格式）
  ```json
  {
    "response_status": {
      "error": "",          // 成功时为空字符串
      "code": 0              // 成功时为 0，失败时为自定义错误码
    },
    "response_data": { }
  }
  ```

## 编码与实现规范

- 文件编码必须为 UTF-8 without BOM
- 遵循 PSR-12（PHP 7.4 兼容）；具体编码风格详见 [coding-style.md](coding-style.md)
- 类名使用 `StudlyCaps`，方法名使用 `camelCase`
- 文件名应与主类名一致（如 `UserService.php`）
- `Control` 层禁止写 SQL；DB 访问仅在 `model`（或 `service`）中通过 Laravel ORM (Eloquent) 完成
- 禁止 `SELECT *`；使用 Laravel ORM (Eloquent) 进行数据访问
- 优先使用 `use` + 自动加载（Composer 或自定义 loader），避免 `require/include`
- **添加/更新 Composer 依赖前需先征得同意**
- 重复逻辑应提取到基类或共享工具类
- 常量定义在相关文件中；禁止硬编码字面量；设计应考虑可扩展性
- 验证必填参数
- 检查函数返回值是否成功
- 添加必要的可追踪日志（含日志级别）
- 注释可使用中文；用户可见的错误信息必须为英文，除非来自 `lang` 包
- 文件行数遵循通用规则（200-400 行为宜，最大 800 行）

## 安全

- 遵循 [../common/security.md](../common/security.md) 中的全部安全要求
- 所有用户输入必须验证；使用 Eloquent 参数绑定防止 SQL 注入
- 生产环境禁止暴露异常详情；用户可见的错误信息必须为英文，除非来自 `lang` 包

## 性能

- 禁止在循环中重复调用 DB/Redis；批量查询、分块处理

## 数据库规范（MySQL 5.7 生产环境）

- 使用 `utf8mb4` 字符集
- 表名以 `t_` 开头，必须有主键，**禁止外键**
- 默认使用 InnoDB，除非有特殊需求
- 表和列必须有 `COMMENT`
- 自增主键类型为 `BIGINT`；日志/追踪表的 `id` 主键使用 `AUTO_INCREMENT`
- 所有列必须 `NOT NULL` 并定义 `DEFAULT` 值
- 避免 `DATETIME`（时区问题）；时间字段使用 `BIGINT`
- 禁止复合主键；使用复合唯一索引代替
- 唯一索引以 `uk_` 或 `uq_` 开头，普通索引以 `idx_` 开头（全小写）；后缀使用表名/字段名或缩写

### 开发环境 SQLite 差异

开发环境使用 SQLite 时，以下 MySQL 规范无法直接适用，需做等效处理：

- `COMMENT`：SQLite 不支持列/表注释，在 migration 文件中用代码注释标注
- `InnoDB`/引擎选择：SQLite 无引擎概念，忽略此项
- `BIGINT AUTO_INCREMENT`：SQLite 使用 `INTEGER PRIMARY KEY AUTOINCREMENT`
- `utf8mb4`：SQLite 默认 UTF-8，无需额外设置
- 其余命名规范（`t_` 前缀、索引命名等）在 SQLite 中同样遵守

## 目录约定

- `action`：API 控制器（入口层），如 `UserControl.php`、`OrderControl.php`
- `service`：核心业务逻辑（可复用），如 `UserService.php`
- `model`：DB 模型 / 数据访问层，如 `UserModel.php`、`OrderModel.php`
- `config`：配置文件，如 `database.php`、`app.php`
- `database`：SQL 文件 / 迁移，如 `20230210_create_users.sql`
- `vendor`：Composer 第三方库
- `lang`：国际化文本（按语言），如 `zh/common.php`、`en/error.php`
- `lib`：共享工具 / 辅助类，如 `Utils.php`、`Encrypt.php`
- `crontab`：定时任务 / CLI 守护进程，如 `notify/`、`clearExpired/`
- `env`：环境文件，如 `.env.local`、`.env.production`
