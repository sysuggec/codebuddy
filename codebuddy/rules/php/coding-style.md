
# PHP 7.4 Coding Style

> This file extends [~/.codebuddy/rules/common/coding-style.md](~/.codebuddy/rules/common/coding-style.md) with PHP 7.4 specific content.

## Standards

- Target **PHP 7.4** language features and constraints
- Use **strict types**: add `declare(strict_types=1);` after `<?php`
- Prefer **type hints** for parameters and return types where supported in PHP 7.4
- Follow **PSR-12** (PHP 7.4 compatible); project-specific exceptions are listed below

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

- **PHP-CS-Fixer** for code formatting (use `.php-cs-fixer.php` if present)
- **PHP_CodeSniffer (PHPCS)** for standards and linting (use `phpcs.xml`/`phpcs.xml.dist` if present)
- **PHPStan** for static analysis (use `phpstan.neon`/`phpstan.neon.dist` if present; level per project)

## Project-Specific Conventions

> 说明：项目约束/架构约定已迁移至 [project-conventions.md](project-conventions.md)。在本项目中使用本规则时，**需同时遵循该文件**。
