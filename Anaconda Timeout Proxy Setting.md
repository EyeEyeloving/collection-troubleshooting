# Anaconda Timeout Proxy Setting

`Warnning!!! The solution provided here is only used for learning. Please refer to the solution in this article while following the local laws and regulations.`
`Warnning!!! The solution provided here is only used for learning. Please refer to the solution in this article while following the local laws and regulations.`
`Warnning!!! The solution provided here is only used for learning. Please refer to the solution in this article while following the local laws and regulations.`

The solution in this article is aimed at installing the virtual environment of anaconda using foreign download sources while mounting a proxy.

Firstly, clear the set domestic mirror download source using

```python
conda config --remove-key channels
```

, restoring the default foreign download source. When you start to create a new virtual environment, you would meet the problem like

```python
Collecting package metadata (current_repodata.json): failed

CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://repo.anaconda.com/pkgs/main/win-64/current_repodata.json>
Elapsed: -

An HTTP error occurred when trying to retrieve this URL.
HTTP errors are often intermittent, and a simple retry will get you on your way.

If your current network has https://www.anaconda.com blocked, please file
a support request with your network engineering team.

'https://repo.anaconda.com/pkgs/main/win-64'
```

One of some available solutions to this question is as follows

```python
conda config --set ssl_verify false
```

Although this approach is not the best, I tried this method to solve my challenge. Then, I encountered the following error.

```python
ProxyError: Conda cannot proceed due to an error in your proxy configuration. Check for typos and other configuration errors in any '.netrc' file in your home directory, any environment variables ending in '_PROXY', and any other system-wide proxy co
```

Many available answers suggested that this error may have resulted from a mistake in the proxy setting. However, I was aware that we could set the proxy oppositely to meet our requirements.

Therefore, I added the setting as follow in the file `.condarc`

```python
proxy_servers:
  http:  socks5h://localhost:xxxxx
  https: socks5h://localhost:xxxxx
```

The approach to obtaining the proxy address can be followed by referring to the [issue](https://github.com/binary-husky/chatgpt_academic/issues/1#issuecomment-1477396626) of another open project.
