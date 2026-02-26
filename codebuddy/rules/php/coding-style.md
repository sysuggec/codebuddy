# PHP 7.4 编码风格

> 本文件扩展自 [../common/coding-style.md](../common/coding-style.md)，补充 PHP 7.4 特定内容。

## 标准

- 目标 **PHP 7.4**，仅使用该版本支持的语言特性
- 使用 **strict types**：在 `<?php` 后添加 `declare(strict_types=1);`
- 尽可能为参数和返回值添加 **类型声明**（PHP 7.4 支持范围内）
- 遵循 **PSR-12**（PHP 7.4 兼容）

## 语言特性 (PHP 7.4)

- 使用 **typed properties** 声明类属性
- 使用 **短数组语法**：`[]`
- 使用 **null 合并**及**null 合并赋值**运算符（`??`、`??=`）
- 短回调使用 **箭头函数**：`fn($x) => $x * 2`
- 禁止使用 PHP 8+ 特性（如 union types、match、构造函数属性提升）

## 不可变性

优先使用不可变值对象。PHP 7.4 没有 `readonly`，通过 `private` 属性 + getter 实现：

```php
<?php

declare(strict_types=1);

final class User
{
    private string $name;
    private string $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getEmail(): string
    {
        return $this->email;
    }
}
```

## 格式化工具

- **PHP-CS-Fixer**：代码格式化（优先使用项目中的 `.php-cs-fixer.php`）
- **PHP_CodeSniffer (PHPCS)**：规范检查（优先使用项目中的 `phpcs.xml` / `phpcs.xml.dist`）
- **PHPStan**：静态分析（优先使用项目中的 `phpstan.neon` / `phpstan.neon.dist`；级别由项目决定）

## 项目专属约定

> 项目约束/架构约定详见 [project-conventions.md](project-conventions.md)。在本项目中使用本规则时，**需同时遵循该文件**。
