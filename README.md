# proxyrequests

By encapsulating net/http again, the syntax of proxyrequests is closer to that of python requests

一、Install

> go get github.com/gamegrd/proxyrequests

二、Demo

1.get

```go
package main

import (
	"fmt"

	"github.com/gamegrd/proxyrequests"
)

func main() {
	resp, err := proxyrequests.Get("https://www.httpbin.org/get?a=1&b=2")
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(resp.Text())

	// Params
	params := proxyrequests.Params{"a": "1", "b": "2"}
	res, err := proxyrequests.Get("https://www.httpbin.org/get", params)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(res.Text())
}
```

2.post

```go
package main

import (
	"fmt"

	"github.com/gamegrd/proxyrequests"
)

func main() {
	// form post
	data := proxyrequests.Data{
		"name": "xiaobulv",
	}
	resp, _ := proxyrequests.Post("https://www.httpbin.org/post", data)
	fmt.Println(resp.Text())

	// json post
	jdata := proxyrequests.Json{"AGE": "131"}
	respb, _ := proxyrequests.Post("https://www.httpbin.org/post", jdata)
	fmt.Println(respb.Text())

}
```

3.put delete patch等

```go
package main

import (
	"fmt"

	"github.com/gamegrd/proxyrequests"
)

func main() {
	// put
	putdata := proxyrequests.Json{"AGE": "131"}
	respp, _ := proxyrequests.Put("https://www.httpbin.org/put", putdata)
	fmt.Println(respp.Text())
}
```

4.auth

```go
package main

import (
	"github.com/gamegrd/proxyrequests"
)

func main() {
	respa, _ := proxyrequests.Get("https://www.httpbin.org/basic-auth/admin/123456", proxyrequests.Auth{"admin", "123456"})
	println(respa.Text())
}
```

5.header, cookie, timeout, proxy, json

```go
package main

import (
	"fmt"

	"github.com/gamegrd/proxyrequests"
)

func main() {
	header := proxyrequests.Header{"user-agent": "hello"}

	data := proxyrequests.Data{
		"name": "xiaobulv",
	}

	// cookie
	ck := proxyrequests.Cookie{"BIDUPSID": "C855441CA6145"}

	// timeout
	timeout := proxyrequests.SetTimeout(10)

	proxy := proxyrequests.Proxy("http://xxx.ip.com")

	resp, _ := proxyrequests.Post("https://www.httpbin.org/post", data, header, ck, timeout)

	// content
	fmt.Println(resp.Content())

	// text
	fmt.Println(resp.Text())

	// response.Json
	m := make(map[string]interface{})
	resp.Json(&m)
	fmt.Println(m["headers"])
	c := m["headers"]
	fmt.Println(c.(map[string]interface{})["Host"])

	fmt.Println(resp.StatusCode)

	// cookie
	fmt.Println(resp.Cookies())
}
```

