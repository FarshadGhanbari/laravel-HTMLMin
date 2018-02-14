Laravel HTMLMin
===============

Laravel HTMLMin is currently maintained by [Raza Mehdi](https://github.com/srmklive), and is a simple HTML minifier for [Laravel 5](http://laravel.com). It utilises Mr Clay's [Minify](https://github.com/mrclay/minify) package to minify entire responses, but can also minify blade at compile time. Feel free to check out the [change log](CHANGELOG.md), [releases](https://github.com/HTMLMin/Laravel-HTMLMin/releases), [license](LICENSE), and [contribution guidelines](CONTRIBUTING.md).

<p align="center">
<a href="https://styleci.io/repos/12090327"><img src="https://styleci.io/repos/12090327/shield" alt="StyleCI Status"></img></a>
<a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square" alt="Software License"></img></a>
<a href="https://github.com/FarshadGhanbari/laravel-HTMLMin/releases"><img src="https://img.shields.io/github/release/HTMLMin/Laravel-HTMLMin.svg?style=flat-square" alt="Latest Version"></img></a>
</p>

## Installation

Laravel HTMLMin requires [PHP](https://php.net) 5.5+. supports all version of Laravel.

## Configuration

Step 1: Create Custom Validation
```bash
$ php artisan make:middleware OptimizeMiddleware
```
After above run command you will find one file on bellow location and you have to write following code:
```bash
<?php

/*
 *
 * (c) Farshad Ghanbari <eng.ghanbari2025@gmail.com>
 *
 */

namespace App\Http\Middleware;

use Closure;

class OptimizeMiddleware
{
    /**
     * Handle an incoming request.
     * (c) Farshad Ghanbari <eng.ghanbari2025@gmail.com>
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        $response = $next($request);
        $buffer = $response->getContent();
        if (strpos($buffer, '<pre>') !== false) {
            $replace = array(
                '/<!--[^\[](.*?)[^\]]-->/s' => '',
                "/<\?php/" => '<?php ',
                "/\r/" => '',
                "/>\n</" => '><',
                "/>\s+\n</" => '><',
                "/>\n\s+</" => '><',
            );
        } else {
            $replace = array(
                '/<!--[^\[](.*?)[^\]]-->/s' => '',
                "/<\?php/" => '<?php ',
                "/\n([\S])/" => '$1',
                "/\r/" => '',
                "/\n/" => '',
                "/\t/" => '',
                "/ +/" => ' ',
            );
        }
        $buffer = preg_replace(array_keys($replace), array_values($replace), $buffer);
        $response->setContent($buffer);
        ini_set('zlib.output_compression', 'On');
        return $response;
    }
}
```


## License

Laravel HTMLMin is licensed under [The MIT License (MIT)](LICENSE).
