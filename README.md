gassa.github.io
===============

<h3>1) Switch to Ruby 1.9.3</h3>
  rvm list                   <br />
   ruby-1.9.3-p545 [ x86_64 ] <br />
=* ruby-2.1.1 [ x86_64 ]      <br />
  rvm use 1.9.3 (Octopress currently only work with Ruby 1.9.3) <br />
  
<h3>2) Edit my post and blog</h3>
  <p>-- Enter into your_local_octopress_repo, <i>cd /home/gassa/my_code/blog_workspace/octopress</i> in my case<p>
  <p>-- Create a new post/blog, <i>rake new_post["title"]</i> </p>
  <p>-- Create a new page, <i>rake new_page[super-awesome]</i> <br />
    &nbsp;&nbsp;&nbsp;&nbsp;For example, <br />
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * rake new_page[super-awesome] generates ./source/super-awesome/index.markdown <br />
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * rake new_page[super-awesome/page.html] generates ./source/super-awesome/page.html<br />
  </p>
 <p> -- Generates posts and pages into the public directory, <i>rake generate</i> <p>
 <p> -- Watches source/ and sass/ for changes and regenerates, <i>rake watch</i> <p>
 <p> -- Watches, and mounts a webserver at http://localhost:4000, <i>rake preview</i> <p>
 
 <h3>3) Introduce file structures </h3>
 <p>
  &nbsp;&nbsp;&nbsp;&nbsp; + public : the results of rake generate.
  &nbsp;&nbsp;&nbsp;&nbsp; + source : the result of rake generate, source code for posts, blogs and pages
 </p>
 
 <h3>3) Deploy sits </h3>
 <p>
    -- Generate your blog, copy the generated files into _deploy/, rake deploy, <br />
    add them to git, commit and push them up to the master branch.              <br />
    -- Push to source branch in repo: <br /> 
      &nbsp;&nbsp;&nbsp;git add . <br />
      &nbsp;&nbsp;&nbsp;git commit -m 'your message' <br />
      &nbsp;&nbsp;&nbsp;git push origin source       <br />
 </p>
 
