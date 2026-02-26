
# PHP 7.4 Coding Style

> This file extends [~/.codebuddy/rules/common/coding-style.md](~/.codebuddy/rules/common/coding-style.md) with PHP 7.4 specific content.

## Standards

- Target **PHP 7.4** language features and constraints
- Use **strict types**: add `declare(strict_types=1);` after `<?php`
- Prefer **type hints** for parameters and return types where supported in PHP 7.4

## Language Features (PHP 7.4)

- Use **typed properties** for class fields
- Use **short array syntax**: `[]`
- Use **null coalescing** and **null coalescing assignment** (`??`, `??=`)
- Use **arrow functions** for short callbacks: `fn($x) => $x * 2`
- Avoid PHP 8+ features (e.g., union types, match, constructor property promotion)

## Immutability

Prefer immutable value objects:

```php
<?php

declare(strict_types=1);

final class User
{
    public string $name;
    public string $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }
}
```

## Formatting

- **PHP-CS-Fixer** for code formatting
- **PHP_CodeSniffer (PHPCS)** for standards and linting
- **PHPStan** for static analysis (level per project)

## Project Constraints & Conventions

1. **Versions**: PHP 7.4, MySQL 5.7
2. **HTTP Methods**: GET + POST (default POST content type is `application/x-www-form-urlencoded` unless specified)
3. **Entry/Exit**: unified at `action/index.php`
4. **Routing**: `?action=模块.方法` → `action/模块Control.php` → `模块Control::方法($request)`
5. **Response Format**: JSON with fixed structure
   ```json
   {
     "response_status": {
       "error": "",          // empty string on success
       "code": 0              // 0 on success, custom error code on failure
     },
     "response_data": { }
   }
   ```
6. **Coding Rules**:
   - Files must be UTF-8 without BOM
   - Enforce PSR-12
   - Class, method, and file names must use CamelCase
   - No SQL in Control layer
   - Prefer `use` + autoloading (Composer or custom loader) over `require/include`
   - **Ask for permission** before adding/updating Composer dependencies
   - Extract duplicate logic into base classes or shared utilities
   - Define constants in related files; avoid hard-coded literals
   - Validate required parameters
   - Check function return values for success
   - Add necessary traceable logs with levels
   - No Chinese error messages unless from language packs
   - Add necessary Chinese comments
   - Disallow `SELECT *`; use Laravel ORM to access DB
   - Avoid hard-coding; design for extensibility
7. **Performance**: avoid repeated DB/Redis calls inside loops; batch queries in chunks
8. **DB Rules**:
   - Use MySQL 5.7 with `utf8mb4` charset
   - Table names must start with `t_`, have a primary key, and **no foreign keys**
   - Use InnoDB by default unless special requirements apply
   - Tables and columns must have `COMMENT`
   - Auto-increment primary key type should be `BIGINT`; for log/trace tables with `id` PK, use `AUTO_INCREMENT`
   - All columns must be `NOT NULL` and define `DEFAULT` values
   - Avoid `DATETIME` due to timezone issues; use `BIGINT` for time fields
   - No composite primary keys; use composite unique indexes instead
   - Unique indexes must start with `uk_` or `uq_`, normal indexes with `idx_` (lowercase only); use table/field names or abbreviations as suffix
9. **Directory Conventions**:
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
9. **Production Errors**: do not expose exceptions in production; no Chinese error messages unless from language packs
