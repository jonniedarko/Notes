###Code Block
find:     <code> \`\`\`(\w+)([\s\S]*?)\`\`\`</code>

replace:  ` [$1]\n$2\n[\\$1] `

###Inline Code block
find:     <code> \`([\s\S]*?)\` </code>

replace:  ` <code>$1</code> `

###URL
find:    ` \[([\s\w]+)\]\((https?:\/\/?[\da-z\.-]+\.[a-z\.]{2,6}[\/\w \%\.-]*\/?)\) `

replace:  ` <a href="$2">$1</a> `

###Heading 1
find:    ` ([\w\s\S]+)\n==+ `

replace: ` <h1>$1</h1> `

###italics
find:    <code> (`\)([^\n^`][^\n^`]*[^\n])(`\) </code>

replace: ` <em>$2</em> `   

###bold
find     ` (\*\*|__)(.+)(\1) `

replace  ` <strong>$2</strong> `

###img
find    ` !\[([\w\s]+)\]\((https?:\/\/?[\da-z\.-]+\.[a-z\.]{2,6}[\/\w \%\.-]*\/?)\) `

replace ` <img title="$1" src="$2"/> `
