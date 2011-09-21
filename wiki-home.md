# The Crypto Project Wiki

This is the main wiki to edit the website content for The Crypto Project located at https://crypto.is.  All website content except the "/" main page and the "/about/" page can be edited through this wiki.

## The Simple Explanation of the Editing Process

*This explanation is for those who are not developers.*

Editing the wiki will not automatically change the content on the live website.  Once an edit is saved, a notification is made that lets the watchers of the repository know of the edits.  Once the modifications are checked by reviewers (anyone) to make sure there is not malicious content, spam, or any other negative alteration the changes will be pushed live and generated into the new website. 

## The More Complicated Explanation

*This explanation is for developers and can be skipped.*

The edits to the wiki are saved in a separate Git repository than that of the live site. Edits that are saved to the wiki trigger a post-commit hook that pushes the content to [This Github Repository][1].  Once the edits are approved, they are merged into the main [Crypto.is Repository][2]. From this point, once the new updates are made, approved, and merged, the build scripts take over and automatically generate the new website from the main repository.

If you are new to github please create an account and follow the article at [How to fork a repo](http://help.github.com/fork-a-repo/).

## How to Edit a Page

Under the section "List of All Current Pages" below, there is a list of links to each page from The Crypto Project website.  All of the web based templates are prefixed with "/md/" which simply is the container directory for the Markdown templates.  All web templates are written in [Markdown][3] syntax.  

So lets say that you wanted to edit the page at [https://crypto.is/interact/mailing_lists/](https://crypto.is/interact/mailing_lists/), you would prepend "/md" to the relative url and check below for "/md/interact/mailing_lists.md" to edit the content from that page.  Once you click the link "/md/interact/mailing_lists.md", you will be taken to the content for that page at [https://wiki.crypto.is/page/md/interact/mailing_lists.md](https://wiki.crypto.is/page/md/interact/mailing_lists.md).  You can edit the site content by clicking the button on the top right titled "Edit Page".

Once your edits have been made, write a brief message below that explains the edits.  At this point, you can view your edits by hitting the "Preview" button or "Save" to make your changes.

## Edit this Page

This homepage is directly editable as well from this wiki.  To edit content from this wiki, go to [/page/wiki-home.md](/page/wiki-home.md)

   [1]: https://github.com/cryptodotis-wiki/crypto.is-docs
   [2]: https://github.com/cryptodotis/crypto.is-docs
   [3]: https://secure.wikimedia.org/wikipedia/en/wiki/Markdown 
