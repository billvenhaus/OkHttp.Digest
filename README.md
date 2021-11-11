OkHttp Digest for Xamarin
================

[![NuGet](https://img.shields.io/nuget/v/OkHttp.Digest.svg)](https://www.nuget.org/packages/OkHttp.Digest/)


Xamarin bindings library for [okhttp-digest](https://github.com/rburgst/okhttp-digest).

The library provides both Basic & Digest authenticators for [OkHttp](https://github.com/square/okhttp).


## Using OkHttp Digest ##

The OkHttp Digest library is available on [Nuget](https://www.nuget.org/packages/OkHttp.Digest/).

If you only need to support the Digest authentication scheme (including auth caching):

```c#
var authenticator = new DigestAuthenticator(new Credentials("username", "pass"));

var authCache = new Dictionary<string, ICachingAuthenticator>();
var client = new OkHttpClient.Builder()
        .Authenticator(new CachingAuthenticatorDecorator(authenticator, authCache))
        .AddInterceptor(new AuthenticationCacheInterceptor(authCache))
        .Build();

var url = "http://www.google.com";
var request = new Request.Builder()
        .Url(url)
        .Get()
        .Build();
var response = client.NewCall(request).Execute();
```

If you want to support both Basic & Digest authentication schemes (including auth caching):

```c#
var credentials = new Credentials("username", "pass");
var basicAuthenticator = new BasicAuthenticator(credentials);
var digestAuthenticator = new DigestAuthenticator(credentials);

// note that all auth schemes should be registered as lowercase!
var authenticator = new DispatchingAuthenticator.Builder()
        .With("digest", digestAuthenticator)
        .With("basic", basicAuthenticator)
        .Build();

var authCache = new Dictionary<string, ICachingAuthenticator>();
var client = new OkHttpClient.Builder()
        .Authenticator(new CachingAuthenticatorDecorator(authenticator, authCache))
        .AddInterceptor(new AuthenticationCacheInterceptor(authCache))
        .Build();
```

License
=======

- **OkHttp.Digest** is licensed under the [Apache License 2.0](https://opensource.org/licenses/Apache-2.0)