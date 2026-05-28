# Agents

This repo contains workflow definitions intended for use with [Yoke](https://github.com/mintybasil/yoke).

Yoke is a Rust daemon that listens for GitHub/GitLab webhook events and dispatches multi-step agent workflows via the Hermes Agent REST API. These workflow TOML files are consumed by Yoke at runtime — they are **not** standalone programs.

See `config.example.toml` for agent and server configuration.
