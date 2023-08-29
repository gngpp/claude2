# Claude to ChatGPT API Adaptation

[简体中文](README.md) | English

## Introduction

This project adapts the chat functionality interface of [Claude](https://claude.ai) to the standard OpenAI API
interfaces.

After starting this project, you can call the interface `http://127.0.0.1:8080/v1/chat/completions` of this project
according to the interface documentation of [v1/chat/completions](https://platform.openai.com/docs/api-reference/chat)
to get the same data structure returned by [OpenAI API](https://platform.openai.com/docs/api-reference/chat). This
facilitates users who have developed based on the interface
of [OpenAI API](https://platform.openai.com/docs/api-reference/chat) to quickly switch over.

## Video Tutorial

[Development Tutorial](https://www.bilibili.com/video/BV1DV4y1q7Dp)

## Runtime Environment

Requires [Go](https://go.dev/dl/) version 1.20 or above.

## Get Source Code

```
git clone https://github.com/gngpp/claude2.git
```

## Run

### IDE

Enter project directory

```
cd claude-to-chatgpt
```

Get dependencies

```shell
# Tidy go.mod
go mod tidy

# Download dependencies in go.mod
go mod download
```

Run

```shell
go run main.go
```

### Other

Use `-c` to specify the configuration file `config-dev.yaml`

Use `-http_proxy` to set `http_proxy` For example `http://127.0.0.1:7890`

```shell
go run main.go -c config-dev.yaml -http_proxy http://127.0.0.1:7890
```

## Configuration

If the configuration file does not exist, the program will create it automatically.

If the configuration information filled in after startup is incorrect, just modify the configuration file directly and save it. The program will automatically reload.

| Configuration  | Description                                                                                                                                                                                                                                                                             | Example Value         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| base-url       | Claude service address, optional                                                                                                                                                                                                                                                        | https://claude.ai     |
| claude         | Claude related configuration                                                                                                                                                                                                                                                            |                       |  
| - session-keys | Unique identifier of current conversation session array, required<br/>Support passing `Bearer sessionKey` in `Header Authorization`<br/>Reference [Authentication](https://platform.openai.com/docs/api-reference/authentication)<br/>Header priority is higher than configuration file | [sk-1, sk-2]          |
| http-proxy     | Proxy configuration, optional,<br/>(Including but not limited to)Note the connectivity in Docker<br/>May need to replace `http://127.0.0.1:7890` with the host IP<br/>Such as `http://192.168.1.100:7890`                                                                               | http://127.0.0.1:7890 |

## Deployment

### Official Image Deployment

Environment Variable

**CLAUDE_SESSION_KEYS**

Set session keys for authentication and authorization of Claude API. Multiple keys can be set, separated by `,`.

**CLAUDE_HTTP_PROXY**

Set the HTTP proxy address used by Claude.

**CLAUDE_BASE_URL**

Set the base address of Claude API, which is the access URL of the service.

```shell
docker pull gngpp/claude2:latest && docker run -p 8787:8787 --name claude-to-chatgpt gngpp/claude2:latest
```

### Manual Compile

You can compile executable files for different platforms.

Windows:

```shell
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build -o claude-to-chatgpt-windows_x64.exe
```

Linux:

```shell
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o claude-to-chatgpt-linux_x64
```

macOS:

```shell
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build -o claude-to-chatgpt-macos_x64
```

### Run

Copy the compiled executable file to the corresponding server directory, grant execution permissions and run it.

## Go Compile Command Parameters

| Parameter   | Description             | Optional Values                           |
|-------------|-------------------------|-------------------------------------------|
| CGO_ENABLED | Whether to enable Cgo   | 0: Disable Cgo<br>1: Enable Cgo (default) |
| GOOS        | Target operating system | linux, windows, darwin etc.               |
| GOARCH      | Target architecture     | amd64, 386, arm etc.                      | 
| go build    | Execute Go compile      |                                           |

Origin：[claude-to-chatgpt](https://github.com/oldweipro/claude-to-chatgpt)
