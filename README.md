# TOML for Cromo

Read [TOML](https://toml.io), the format of many files of settings, from a [Cromo](https://cromolang.org) program. Written in Cromo: it brings no library with it.

```sh
cromo get github.com/cromolang/toml
```

```cromo
import std.fs
import toml

struct Server {
    string host
    i64 port = 80
}

struct Settings {
    string title
    Server server
    list<string> tags
}

i32 main() {
    Settings s = toml.decode<Settings>(fs.readText("settings.toml"))
    println("{} on {}:{}", s.title, s.server.host, s.server.port)
    ret 0
}
```

```toml
title = "Shop"
tags = ["web", "eu"]

[server]
host = "localhost"
port = 8080
```

## What it has

- `toml.decode<T>(text)` reads a text into a struct of the program, as `json.decode` does for JSON: a table gives a struct or a map, an array a list, and a key that is missing leaves the default value of its field.
- `toml.parse(text)` gives a `json.Value`, for a text whose shape is not known: `v.get("server").get("port").asInt()`.

It reads TOML 1.0: tables and arrays of tables, keys with dots, the four kinds of strings, integers also in hexadecimal, octal and binary, floats, booleans, arrays and inline tables.

A text that is not TOML is an `Error` with its line, as `toml: line 4: the key 'port' is written twice`.

## Limits of this version

- A date or a time is given as its text, as `"2026-11-16T10:30:00Z"`: `std.time` reads it with `time.parseIso`.
- It reads TOML: it does not write it.
- `inf` and `nan` are read, but a value with them cannot be written as JSON.

## The versions

| This package | Cromo |
|---|---|
| v0.1.0 | 0.90 or later |

## Developing it

```sh
cromo test toml.cro
```

## License

MIT: see `LICENSE`.
