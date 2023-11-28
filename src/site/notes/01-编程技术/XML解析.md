---
{"dg-publish":true,"permalink":"/01-编程技术/XML解析/","dgPassFrontmatter":true,"created":"2023-10-27T08:59:53.232+08:00","updated":"2023-11-23T15:29:16.000+08:00"}
---

#xml
# xml中节点带冒号:

``` xml
<d:multistatus xmlns:d="DAV:" xmlns:s="http://ns.jianguoyun.com">
  <d:response>
    <d:href>/dav/</d:href>
    <d:propstat>
      <d:prop>
        <d:getcontenttype>httpd/unix-directory</d:getcontenttype>
        <d:displayname>dav</d:displayname>
        <d:owner>472189065@qq.com</d:owner>
        <d:resourcetype>
          <d:collection />
        </d:resourcetype>
        <d:getcontentlength>0</d:getcontentlength>
        <d:getlastmodified>Tue, 05 Sep 2023 02:08:55 GMT</d:getlastmodified>
        <d:current-user-privilege-set>
          <d:privilege>
            <d:read />
          </d:privilege>
        </d:current-user-privilege-set>
      </d:prop>
      <d:status>HTTP/1.1 200 OK</d:status>
    </d:propstat>
  </d:response>
</d:multistatus>

```

冒号其实是命名空间的意思，参照声明里的 ``xmlns:d="DAV:"``
在解析的时候，把 ``d:`` 替换为 ``DAV:``