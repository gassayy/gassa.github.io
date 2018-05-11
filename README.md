This blog is built with jekyll.

### Jekyll Cheet Sheet:

> jekyll build
- The current folder will be generated into ./_site, which contains generated static web html, css, js and etc.

> jekyll build --destination <destination>
- The current folder will be generated into <destination>

> jekyll build --source <source> --destination <destination>
- The <source> folder will be generated into <destination>

> jekyll build --watch
- The current folder will be generated into ./_site, watched for changes, and regenerated automatically.

> jekll serve
Preview the local site at http://127.0.0.1:4000

### How to Create A Post:
To create a new post, all you need to do is create a new file in the _posts directory. The newly created post has to follow Jekyll's naming conventions: YEAR-MONTH-DAT-title.MARKUP, Where YEAR is a four-digit number, MONTH and DAY are both two-digit numbers, and MARKUP is the file extension representing the format used in the file.

For example, 2011-12-31-new-years-eve-is-awesome.md or 2012-09-12-how-to-write-a-blog.

CSS Ref.
* CSS font awesome icon: http://astronautweb.co/snippet/font-awesome/
