# HyperLisp
HyperLisp is the language of the new web.
### XML
The web has historically used HTML/XML as its primary data interchange format. In HyperLisp, content of the form `<ELEMENT PROPERTY=VALUE>CONTENT</ELEMENT>` becomes `(ELEMENT
	(PROERTY VALUE) CONTENT)`. Here's an example.
```html
<!-- HTML -->
<head>
	<title>This is my page!</title>
</head>

<body class="foo" id="bar">
	This is my content.
</body>
```
This becomes
```scheme
(defcontent main
	(head
		(content
			(title "This is my page!")))
	(body
		(style 'foo)
		(id 'bar)
		(content
			"This is my content!")))
```
### CSS
The web has historically used CSS to define rendering information on top of XML. In HyperLisp, content of the form `rule: value;` becomes content of the form `(rule value)`.
```css
p {
	text-align: center;
	color: red;
} 
```
This becomes
```scheme
(defstyle 'p
	(text-align center)
	(color red))
```
### JavaScript
The web has historically used JavaScript for most of its imperative frontend content. Instead, these `defstyle` and `defcontent` macros can be built upon D-Lisp.
```javascript
let square = (x) => {
	return x * x;
};
```
Becomes
```scheme
(defun square(x)
	(* x x))
```