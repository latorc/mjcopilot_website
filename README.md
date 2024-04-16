# mjcopilot.com

Website source code
Built using Jekyll
template: 


### To develope:

1. Install Ruby and Jekyll
2. Install dependencies

```
bundle install
```

3. run local server:

```
bundle exec jekyll s
```

### Notes:

Index Page: using layout and include showcase.html
Help Page: help.md

### update.mjcopilot.com

```js
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  // 基础源站点URL
  const sourceSite = 'https://github.com/latorc/mjcopilot_website/raw/main/update';

  // 获取原始请求的URL
  const url = new URL(request.url);

  // 构建新的请求URL，结合源站点和用户请求的路径
  const newURL = `${sourceSite}${url.pathname}${url.search}`;

  // 使用fetch请求源站点
  const response = await fetch(newURL, {
    method: request.method,
    headers: request.headers,
    body: request.method === "GET" || request.method === "HEAD" ? undefined : request.body
    // 注意: 对于GET和HEAD请求，不应包含body
  });

  // 返回源站点的响应给用户
  return new Response(response.body, {
    status: response.status,
    statusText: response.statusText,
    headers: response.headers
  });
}
```

### download.mjcopilot.com

```js
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const apiUrl = `https://api.github.com/repos/latorc/MahjongCopilot/releases/latest`;

  // Extract the User-Agent from the incoming request
  const userAgent = request.headers.get('User-Agent');

  // Fetch the latest release data from GitHub, including the forwarded User-Agent header
  const response = await fetch(apiUrl, {
    headers: {
      'User-Agent': userAgent // Forward the User-Agent from the incoming request
    }
  });

  if (!response.ok) {
    // Clone the response stream to be able to read it and send it as part of our Response
    const originalResponse = await response.clone().text();
    return new Response(originalResponse, {
      status: response.status,
      statusText: response.statusText,
      headers: response.headers
    });
  }

  const releaseData = await response.json();

  // Find the asset with the specific name
  const asset = releaseData.assets.find(asset => asset.name === "MahjongCopilot.windows.7z");

  if (!asset) {
    return new Response('File not found in the latest release', { status: 404 });
  }

  // Fetch the actual file content using the asset's download URL
  const fileResponse = await fetch(asset.browser_download_url, {
    headers: {
      'User-Agent': userAgent // Important to maintain consistency
    }
  });

  if (!fileResponse.ok) {
    return new Response('Failed to download file', {
      status: fileResponse.status,
      statusText: fileResponse.statusText
    });
  }

  // Forward the file content directly to the user
  return new Response(fileResponse.body, {
    status: 200,
    headers: {
      'Content-Type': 'application/octet-stream', // You may want to specify the correct MIME type
      'Content-Disposition': `attachment; filename="${asset.name}"`
    }
  });
}

```

