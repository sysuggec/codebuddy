# PHP 项目约束与架构约定

> 本文件为项目专属规范，需与 [coding-style.md](coding-style.md) **同时使用**。

## 说明

以下为项目约束/架构约定，若与通用规范冲突，以本文件为准。

## 环境与版本

- **Versions**: PHP 7.4, MySQL 5.7 ，开发环境使用sqlite代替
- **Dependency**: Laravel ORM (Eloquent)，所有 DB 访问必须通过它

## 接口与路由

- **HTTP Methods**: GET + POST (default POST content type is `application/x-www-form-urlencoded` unless specified)
- **Entry/Exit**: unified at `action/index.php`
- **Routing**: `?action=模块.方法` → `action/模块Control.php` → `模块Control::方法($request)`
- **Response Format**: JSON with fixed structure
  ```json
  {
    "response_status": {
      "error": "",          // empty string on success
      "code": 0              // 0 on success, custom error code on failure
    },
    "response_data": { }
  }
  ```

## 编码与实现规范

- Files must be UTF-8 without BOM
- Follow PSR-12 (PHP 7.4 compatible); project exceptions are listed here
- Class names use `StudlyCaps`, method names use `camelCase`
- File names should match primary class name (e.g. `UserService.php`)
- No SQL in `Control` layer; DB access only in `model` (or `service`) via Laravel ORM (Eloquent)
- Disallow `SELECT *`; use Laravel ORM (Eloquent) for data access
- Prefer `use` + autoloading (Composer or custom loader) over `require/include`
- **Ask for permission** before adding/updating Composer dependencies
- Extract duplicate logic into base classes or shared utilities
- Define constants in related files; avoid hard-coded literals; design for extensibility
- Validate required parameters
- Check function return values for success
- Add necessary traceable logs with levels
- Comments may be Chinese when helpful; user-visible error messages must be English unless from `lang` packs

## 性能

- avoid repeated DB/Redis calls inside loops; batch queries in chunks

## 数据库规范

- Use MySQL 5.7 with `utf8mb4` charset
- Table names must start with `t_`, have a primary key, and **no foreign keys**
- Use InnoDB by default unless special requirements apply
- Tables and columns must have `COMMENT`
- Auto-increment primary key type should be `BIGINT`; for log/trace tables with `id` PK, use `AUTO_INCREMENT`
- All columns must be `NOT NULL` and define `DEFAULT` values
- Avoid `DATETIME` due to timezone issues; use `BIGINT` for time fields
- No composite primary keys; use composite unique indexes instead
- Unique indexes must start with `uk_` or `uq_`, normal indexes with `idx_` (lowercase only); use table/field names or abbreviations as suffix

## 目录约定

- `action`: API controllers (entry layer), e.g. `UserControl.php`, `OrderControl.php`
- `service`: core business logic (reusable), e.g. `UserService.php`
- `model`: DB model / data access layer, e.g. `UserModel.php`, `OrderModel.php`
- `config`: config files, e.g. `database.php`, `app.php`
- `database`: SQL files / migrations, e.g. `20230210_create_users.sql`
- `vendor`: Composer third-party libraries
- `lang`: i18n text by locale, e.g. `zh/common.php`, `en/error.php`
- `lib`: shared utils / helpers, e.g. `Utils.php`, `Encrypt.php`
- `crontab`: scheduled tasks / CLI daemons, e.g. `notify/`, `clearExpired/`
- `env`: environment files, e.g. `.env.local`, `.env.production`

## 生产错误处理

- do not expose exceptions in production; user-visible error messages must be English unless from `lang` packs
