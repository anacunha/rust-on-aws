# Rust on AWS

## Prerequisites

You will need to create an AWS accounts and setup your access keys. Follow the instructions [here](https://docs.aws.amazon.com/sdk-for-rust/latest/dg/getting-started.html#getting-started-prerequisites).

## AWS Rust SDK

### Upload file to Amazon S3 bucket

1. Create new project

```shell
cargo new aws_rust_sdk
```

2. Add dependencies (tokio, aws-config, aws-sdk-s3)

```shell
cargo add tokio@1 --features full
cargo add aws-config
cargo add aws-sdk-s3
```

Your dependencies on `Cargo.toml` will look like this:

```toml
[dependencies]
aws-config = "0.48.0"
aws-sdk-s3 = "0.18.0"
tokio = { version = "1", features = ["full"] }
```

3. We will use [tokio](https://tokio.rs) as our asynchronous runtime:

```rust
#[tokio::main]
async fn main() {
  // Our code will go here
}
```

4. Create an `SdkConfig` by loading the default configuration from the environment:

```rust
let config = aws_config::load_from_env().await;
```

5. Create and `aws_sdk_s3::Client`:

```rust
let client = aws_sdk_s3::Client::new(&config);
```

6. Create a `ByteStream` from the file that will be uploaded:

```rust
let body = aws_sdk_s3::types::ByteStream::from_path(std::path::Path::new("test.txt")).await;
```

7. Create a [`PutObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html) request:

```rust
let response = client
    .put_object()
    .bucket("bucket-name")
    .key("key")
    .body(body.unwrap())
    .send()
    .await?;
```

8. Get the object version:

```rust
let version = response.version_id().unwrap();
println!("Uploaded file version {}", version);
```

Our entire `main.rs` file will look like this:

```rust
use aws_sdk_s3::{types::ByteStream, Client, Error};
use std::path::Path;

#[tokio::main]
async fn main() -> Result<(), Error> {
    let config = aws_config::load_from_env().await;
    let client = Client::new(&config);

    let body = ByteStream::from_path(Path::new("test.txt")).await;

    let response = client
        .put_object()
        .bucket("tech-day-ifood")
        .key("demo")
        .body(body.unwrap())
        .send()
        .await?;

    let version = response.version_id().unwrap();
    println!("Uploaded file version {}", version);

    Ok(())
}
```

- [AWS Rust SDK](https://aws.amazon.com/sdk-for-rust/)
- [Developer Guide](https://docs.aws.amazon.com/sdk-for-rust/latest/dg/welcome.html)
- [GitHub repo](https://github.com/awslabs/aws-sdk-rust)
- [Exemplos de código](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview)
- [Roadmap](https://github.com/orgs/awslabs/projects/50/views/1)

## AWS Lambda Rust Runtime

1. Install [cargo-lambda](https://github.com/cargo-lambda/cargo-lambda)

```shell
brew tap cargo-lambda/cargo-lambda
brew install cargo-lambda
```

2. Create a function

```shell
cargo lambda new FUNCTION_NAME
```

3. Build

```shell
cargo lambda build --release
cargo lambda build --release --arm64
```

4. Deploy

```shell
cargo lambda deploy \
  --iam-role arn:aws:iam::XXXXXXXXXXXXX:role/your_lambda_execution_role
```

5. Test

```shell
cargo lambda invoke --remote \
  --data-ascii '{"command": "hi"}' \
  --output-format json \
  FUNCTION_NAME
```

- [Rust Runtime for AWS Lambda](https://github.com/awslabs/aws-lambda-rust-runtime)
- [cargo-lambda](https://github.com/cargo-lambda/cargo-lambda)

## Links

- [Rust on AWS](https://aws.amazon.com/developer/language/rust/)

### Blogs

- [Innovating with Rust](https://aws.amazon.com/blogs/opensource/innovating-with-rust/)
- [A New AWS SDK for Rust – Alpha Launch](https://aws.amazon.com/blogs/developer/a-new-aws-sdk-for-rust-alpha-launch/)
- [How our AWS Rust team will contribute to Rust’s future successes](https://aws.amazon.com/blogs/opensource/how-our-aws-rust-team-will-contribute-to-rusts-future-successes/)
- [Why AWS loves Rust, and how we’d like to help](https://aws.amazon.com/blogs/opensource/why-aws-loves-rust-and-how-wed-like-to-help/)
- [Rust Runtime for AWS Lambda](https://aws.amazon.com/blogs/opensource/rust-runtime-for-aws-lambda/)

### Videos

- [Rust Linz June 2022 - How AWS is building the Rust SDK and how you can use it today (Zelda Hessler)](https://youtu.be/N0XMjokwTIM)
- [AWS Summit San Francisco 2022 - Sustainability with Rust](https://youtu.be/BHRLbMCpmtY)
- [AWS re:Invent 2021 - Building with the new AWS SDKs for Rust, Kotlin, and Swift](https://youtu.be/Nhk1K1AjYvg)
- [AWS re:Invent 2021 - Using Rust to minimize environmental impact](https://youtu.be/yQZaBtUjQ1w)
- [AWS re:Invent 2020: Next-gen networking infrastructure with Rust and Tokio](https://youtu.be/MZyleK8elPk)
