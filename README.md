# simple-markdown-formatter

A **Customizable Markdown Library** for rendering Markdown in javascript. It converts markdown to html.

Custom markdown rules for your application is also possible without any regex learning. Just follow the simple guidelines shown bellow.   

## Getting started
### Default Configuration:

* '\_italic_' will result in '_italic_'.
* '\*\*bold**' will result in '**bold**'.
* '\[Title](link)' will result in [Title](link).
* '- bullet 1\r - bullet2\r - bulltet 3' will result in following way
   ```
    . bullet 1 
    . bullet2
    . bullet 3
    ```
* '1\. numbered 1\r 2. numbered2\n 3. numbered 3' will result in following way
    ```
    1. numbered 1 
    2. numbered2
    3. numbered 3
    ```

### Custom Configurations:

To replace or to be added with default config.
   
*       {
            type: 'italic',
            styles:{
                start: "<i>",
                end: "</i>"},
            pattern: ['-'],
            patternType: 'symmetric',
            groups:1
        }
	
    The above pattern will render '\-italic-' in '_italic_'.

*       {
    		type: 'bullet', 
            styles:{
                start: "<ul>",
                end: "</ul>"},
            pattern: ['$', '\\r|\\n'],
            patternType: 'start-end',
            groups: 1
    	},

    The above pattern will convert 'bullet list $ Item 1.1\r$ Item -2.2-\r$ Item 3\r' in following way: 
    
    ```
    bullet list
    . Item 1.1
    . Item -2.2-
    . Item 3
    ```
    
*   One can create custom markdown based on project need and render it within text.
     
## Usage

Add the script to your document and call it like below.

```js
<script type="text/javascript" src="markdown-formatter.min.js"></script>

outputHtml = MarkdownFormatter.formatText(text);

```
**To use with Adaptive cards**: Call the formatText inside onProcessMarkdown method.
```js
adaptiveCard.constructor.onProcessMarkdown = function(text, result) {
	result.outputHtml = MarkdownFormatter.formatText(text);
	result.didProcess = true;
}

```

**Extra Features**: User can also send their custom regex and styles also to apply on text.
```js
//[Optional] user's custom config to be added to default configs
MarkdownFormatter.setCustomMarkdownRegex([{
        type: 'bullet', 
        styles:{
            start: "<ul>",
            end: "</ul>"},
        pattern: ['$', '\\r|\\n'],
        patternType: 'start-end',
        groups: 1
    },
    {
        type: 'italic',
        styles:{
            start: "<i>",
            end: "</i>"},
        pattern: ['-'],
        patternType: 'symmetric',
        groups:1,
    }]);

```

NOTE: 

```js
// formatter default pattern configs which user does not have to pass to markdown
MD_FORMATTER_CONFIG = [
        {
            type: 'numbered',
            styles:{
                start: "<ol>",
                end: "</ol>"},
            pattern: ["\\d.", '\\r|\\n'],
            patternType: 'start-end',
            groups: 1,
        },  
        {
            type: 'bullet',
            styles:{
                start: "<ul>",
                end: "</ul>"},
            pattern: ['-', '\\r|\\n'],
            patternType: 'start-end',
            groups: 1,
        },  
		{
			type: 'bold',
            styles:{
            start: "<b>",
            end: "</b>"},
			pattern: ['**'],
			patternType: 'symmetric',
			groups: 1,
		},
		{
			type: 'italic',
            styles:{
                start: "<i>",
                end: "</i>"},
			pattern: ['_'],
			patternType: 'symmetric',
			groups: 1,
		},
		{
			type: 'hyperlink',
            styles: {
                html:"<a href='{1}'>{0}</a>"
            },
			pattern: ['[]()'],
			patternType: 'asymmetric',
			groups: 2,
		},
    ];
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
