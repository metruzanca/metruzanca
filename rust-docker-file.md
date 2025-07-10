---
title: Fast dockerfile for Rust apps
tags:
  - rust
  - docker
  - dockerfile
timestamp: 2025-07-10T00:06:18
publish: true
---

```dockerfile
FROM lukemathwalker/cargo-chef:latest-rust-1.87.0 AS chef
WORKDIR app

FROM chef AS planner
COPY . .
RUN cargo chef prepare --recipe-path recipe.json

FROM chef AS builder
COPY --from=planner /app/recipe.json recipe.json
# Build dependencies - this is the caching Docker layer!
RUN cargo chef cook --release --recipe-path recipe.json --target=x86_64-unknown-linux-musl
# Build application
COPY . .
RUN cargo build --release --target=x86_64-unknown-linux-musl --bin app

# We do not need the Rust toolchain to run the binary!
FROM rust:1.87-alpine AS runtime
WORKDIR app
COPY --from=builder /app/target/release/<my-app> /usr/local/bin

CMD ["/usr/local/bin/<my-app>"]
```

---

## Sources

After doing some trial and error myself, I found this blog post that was very interesting [Why is the Rust compiler so slow?](https://sharnoff.io/blog/why-rust-compiler-slow). Most of it is not relevant, but it did expose me to [cargo-chef](https://github.com/LukeMathWalker/cargo-chef) which is an important part of the final dockerfile. This post from Luca Palmieri "[5x Faster Rust Docker Builds with cargo-chef](https://lpalmieri.com/posts/fast-rust-docker-builds/)" helped me get the final dockerfile setup.
