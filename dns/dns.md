
> Stability: 2 - Stable
>稳定性：2 - 稳定
The `dns` module contains functions belonging to two different categories:
DNS模块包含两个不同类型的功能
1) Functions that use the underlying operating system facilities to perform
name resolution, and that do not necessarily perform any network communication.
This category contains only one function: [`dns.lookup()`][]. **Developers
looking to perform name resolution in the same way that other applications on
the same operating system behave should use [`dns.lookup()`][].**

1）使用底层操作系统执行名称解析，而不必执行任何网络通信的功能。这一类型仅包含一个函数：dns.lookup()。
**希望在相同操作系统上不同应用上使用相同方法进行名称解析的开发者可以使用dns.lookup()函数**
For example, looking up `iana.org`.

比如，参见‘’iana,org:

const dns = require('dns');

dns.lookup('nodejs.org', (err, addresses, family) => {
  console.log('addresses:', addresses);
});
// address: "192.0.43.8" family: IPv4
```

2) Functions that connect to an actual DNS server to perform name resolution,
and that _always_ use the network to perform DNS queries. This category
contains all functions in the `dns` module _except_ [`dns.lookup()`][]. These
functions do not use the same set of configuration files used by
[`dns.lookup()`][] (e.g. `/etc/hosts`). These functions should be used by
developers who do not want to use the underlying operating system's facilities
for name resolution, and instead want to _always_ perform DNS queries.

2)连接到实际的DNS服务器执行名称解析并且一直使用网络执行DNS查询的功能。这一类型除了dns.lookup()函数包含DNS模块中的所用功能。
这些函数不能使用由dns.lookup()函数配置文件相同的配置（e.g. /etc/hosts）。这些函数的应用是供那些不想使用底层操作系统而总是想通过DNS查询来执行名称解析的开发者使用的。
Below is an example that resolves `'archive.org'` then reverse resolves the IP
addresses that are returned.

下面是一个例子，解决‘archive.org’然后反相解析返回的IP地址。
```js
const dns = require('dns');

dns.resolve4('archive.org', (err, addresses) => {
  if (err) throw err;

  console.log(`addresses: ${JSON.stringify(addresses)}`);

  addresses.forEach((a) => {
    dns.reverse(a, (err, hostnames) => {
      if (err) {
        throw err;
      }
      console.log(`reverse for ${a}: ${JSON.stringify(hostnames)}`);
    });
  });
});
```

There are subtle consequences in choosing one over the other, please consult
the [Implementation considerations section][] for more information.

