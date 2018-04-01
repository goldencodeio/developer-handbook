# PHPStorm

[*PhpStorm*](//www.jetbrains.com/phpstorm/) — коммерческая кросс-платформенная интегрированная среда разработки для PHP. Разрабатывается компанией JetBrains на основе платформы IntelliJ IDEA.

## Настройка параметров PHPStorm

- Editor
	- General
		- Appearance
			- Show line numbers
			- Show whitespaces
		- Smart Keys
			- Use 'CamelHumps' words
- Languages & Frameworks
	- JavaScript
		- JavaScript language version = ECMAScript 6
		- Code Quality Tools
			- ESLint
				- Enable
				- Set 'Node interpreter'
				- Set 'ESLint package'
				- Configuration file = Automatic search
	- PHP
		- Set interpreter from [Nginx Stack](./3-software/3.1-local-server.md)
		- Composer
			- Set path to Composer.phar from [Nginx Stack](./3-software/3.1-local-server.md)
	- Node.js and NPM
		- Set Node interpreter


## Установка необходимых плагинов

- *EditorConfig* - [EditorConfig](./1-standards/1.5-editorconfig.md)
- *Pug (ex-Jade)* - Поддержка подвсетки Pug (шаблонизатор)
