---
isChild: true
anchor:  password_hashing
title: 密码哈希
---

## 密码哈希 {#password_hashing_title}

每个人在建构 PHP 应用时终究都会加入用户登录的模块。用户的帐号及密码会被储存在数据库中，在登录时用来验证用户。

It is important that you properly [_hash_][3] passwords before storing them. Hashing and encrypting are [two very different things][7]
that often get confused.

Hashing is an irreversible, one-way function. This produces a fixed-length string that cannot be feasibly reversed.
This means you can compare a hash against another to determine if they both came from the same source string, but you
cannot determine the original string. If passwords are not hashed and your database is accessed by an unauthorized
third-party, all user accounts are now compromised.

Unlike hashing, encryption is reversible (provided you have the key). Encryption is useful in other areas, but is a poor
strategy for securely storing passwords.


密码应该单独被  [加盐处理][5] ，加盐值指的是在哈希之前先加入随机子串。以此来防范「字典破解」或者「彩虹碰撞」（一个可以保存了通用哈希后的密码数据库，可用来逆向推出密码）。

哈希和加盐非常重要，因为很多情况下，用户会在不同的服务中选择使用同一个密码，密码的安全性很低。

Additionally, you should use [a specialized _password hashing_ algoithm][6] rather than fast, general-purpose
cryptographic hash function (e.g. SHA256). The short list of acceptable password hashing algorithms (as of June 2018)
to use are:

* Argon2 (available in PHP 7.2 and newer)
* Scrypt
* **Bcrypt** (PHP provides this one for you; see below)
* PBKDF2 with HMAC-SHA256 or HMAC-SHA512


值得庆幸的是，在 PHP 中这些很容易做到。

**使用 `password_hash` 来哈希密码**

`password_hash` 函数在 PHP 5.5 时被引入。 此函数现在使用的是目前 PHP 所支持的最强大的加密算法 BCrypt 。 当然，此函数未来会支持更多的加密算法。 `password_compat` 库的出现是为了提供对 PHP >= 5.3.7 版本的支持。

在下面例子中，我们哈希一个字符串，然后和新的哈希值对比。因为我们使用的两个字符串是不同的（'secret-password' 与 'bad-password'），所以登录失败。

{% highlight php %}
<?php
require 'password.php';

$passwordHash = password_hash('secret-password', PASSWORD_DEFAULT);

if (password_verify('bad-password', $passwordHash)) {
    // Correct Password
} else {
    // Wrong password
}
{% endhighlight %}  

`password_hash()` 已经帮你处理好了加盐。加进去的随机子串通过加密算法自动保存着，成为哈希的一部分。`password_verify()` 会把随机子串从中提取，所以你不必使用另一个数据库来记录这些随机子串。


* [了解 `password_hash()`] [1]
* [PHP >= 5.3.7 && < 5.5 的 `password_compat`] [2]
* [了解密码学中的哈希] [3]
* [学习下加盐] [5]
* [PHP `password_hash()` RFC] [4]

[1]: https://secure.php.net/function.password-hash
[2]: https://github.com/ircmaxell/password_compat
[3]: https://wikipedia.org/wiki/Cryptographic_hash_function
[4]: https://wiki.php.net/rfc/password_hash
[5]: https://wikipedia.org/wiki/Salt_(cryptography)
[6]: https://paragonie.com/blog/2016/02/how-safely-store-password-in-2016
[7]: https://paragonie.com/blog/2015/08/you-wouldnt-base64-a-password-cryptography-decoded
