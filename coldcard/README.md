# Coldcard Interface Library

`coldcard` is a library for interfacing with the [Coldcard](https://coldcard.com/) hardware wallet.

## Usage

```rust
use coldcard::protocol;

// create an API instance
let mut api = coldcard::Api::new()?;

// detect all connected Coldcards
let serials = api.detect()?;

// get the first serial and open it
let first = serials.into_iter().next().unwrap();
let (mut cc, master_xpub) = api.open(first, None).unwrap();

// set a passphrase
coldcard.set_passphrase(protocol::Passphrase::new("secret")?)?;

// after the user confirms
let xpub = coldcard.get_passphrase_done()?;

if let Some(xpub) = xpub {
    println!("The new XPUB is: {}", xpub);
}

// secure logout
coldcard.logout()?;
```

## Linux Specific Instructions

In order to be able to detect a Coldcard device on a Linux system, [51-coinkite.rules](../51-coinkite.rules) must be placed in `/etc/udev/rules.d/`.

Two mutually exclusive HID backends are supported and can be turned on using the following features:

* `linux-static-hidraw` (default)
* `linux-static-libusb` (potential issues with [unclear error messages](https://github.com/libusb/hidapi/blob/f2e2b5b4d4caa9942ad2cd594da00956b51f0ca6/libusb/hid.c#L1637))

## Logging

The `log` feature enables logging using the `log` crate. Disabled by default. Use judiciously as logging can leak details into the environment.

## CLI

This project also offers a CLI tool. See the project's own crate for more information.

Install it with:

```bash
$ cargo install coldcard-cli
```

## Contributing

Contributions are welcome. Before making large changes, please open an issue first.

## Disclaimer

This is not an official project and comes with no warranty whatsoever.