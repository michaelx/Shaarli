## Features

### Shaares

Content posted to Shaarli is separated in items called _Shaares_. For each Shaare,
you can customize the following aspects:

- URL to link to
- Title
- Free-text description
- Tags
- Public/private status

While logged in to your Shaarli, you can add, edit or delete Shaares:

- Using the **+Shaare** button: enter the URL you want to share, click `Add link`, and fill in the details of your Shaare, and `Save`
- Using the [Bookmarklet](https://en.wikipedia.org/wiki/Bookmarklet): drag the `✚Shaare link` button from the `Tools` page to your browser's bookmarks bar, click it to share the current page.
- Using [apps and browser addons](Community-&-Related-software.md#mobile-apps)
- Using the [REST API](https://shaarli.github.io/api-documentation/)
- Any Shaare can edited by clicking its ![](images/edit_icon.png) `Edit` button.


### Public/private shaares

Additional filter buttons can be found at the top left of the link list **only when logged in**:

- **Only show private links:** Private shares can be searched by clicking the `only show private links` toggle button top left of the link list (only when logged in)


### Permalinks

Permalinks fixed, short links attached to each Shaare. Editing a Shaare will not change it's permalink, each permalink always points to the latest revision of a Shaare.


### Text-only (note) shaares

Shaarli can be used as a minimal blog, notepad, pastebin...: While adding or editing a Shaare, leave the URL field blank to create a text-only ("note") post. This allows you to post any kind of text content, such as blog articles, private or public notes, snippets... There is no character limit! You can access your post from its permalink.


### Text/tags search

- **Plain text search:** Use `Search text` to search in all fields of all links (Title, URL, Description...). Use double-quotes (example `"exact search"`) to search for the exact expression.  
- **Tags search:** `Filter by tags` restricts displayed links to entries tagged with one or multiple tags (use space to separate tags).
- **Hidden tags:** tags starting with a dot `.` (example `.secret`) are private. They can only be seen and searched when logged in.
- **Exclude text/tags:** Use the `-` operator before a word or tag to exclude Shaares matching this word from search results (`NOT` operator).
- **Untagged links:** Shaares without tags can be searched by clicking the `untagged` toggle button top left of the link list (only when logged in).


Both exclude patterns and exact searches can be combined with normal searches (example `"exact search" term otherterm -notthis "very exact" stuff -notagain`). Only AND (and NOT) search is currrently supported.


### Tag cloud

The `Tag cloud` page diplays a "cloud" or list view of all tags in your Shaarli (most frequently used tags are displayed with a bigger font size)


- **Tags list:** click on `Most used` or `Alphabetical` to display tags as a list. You can also edit/delete tags for this page.
- Click on any tag to search all Shaares matching this tag.
- **Filtering the tag cloud/list:** Click on the counter next to a tag to show other tags of shaares with this tag. Repeat this any number of times to further filter the tag cloud. Click `List all links with those tags` to display Shaares matching your current tag filter set.



### RSS feeds

RSS/ATOM feeds feeds are available (in ATOM with `?do=atom` and RSS with `?do=RSS`)

- **Filtering RSS feeds:** RSS feeds and picture wall can also be restricted to only return items matching a text/tag search. For example, search for `photography` (text or tags) in Shaarli, then click the `RSS Feed` button. A feed with only matching results is displayed.
- Add the `&nb` parameter in feed URLs to specify the number of Shaares you want in a feed (default if not specified: `50`). The keyword `all` is available if you want everything.
- Add the `&permalinks` parameter in feed URLs to point permalinks to the corresponding shaare instead of the shaared URL

![](images/rss-filter-1.png) ![](images/rss-filter-2.png)

```bash
# examples
https://shaarli.mydomain.org/?do=atom&permalinks
https://shaarli.mydomain.org/?do=atom&permalinks&nb=42
https://shaarli.mydomain.org/?do=atom&permalinks&nb=all
https://shaarli.mydomain.org/?do=rss&searchtags=nature
https://shaarli.mydomain.org/links/?do=picwall&searchterm=poney
```


### Picture wall

- The picture wall can be filtered by text or tags search in the same way as [RSS feeds](#rss-feeds)


### Import/export

To **export links as a HTML file**, under _Tools > Export_, choose:

- `Export all` to export both public and private links
- `Export public` to export public links only
- `Export private` to export private links only

Restore by using the `Import` feature.

- These exports contain the full data (URL, title, tags, date, description, public/private status of your Shaares)
- They can also be imported to your web browser bookmarks. 

