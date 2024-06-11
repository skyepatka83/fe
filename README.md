
<img src="https://raw.githubusercontent.com/ethereum/fe/master/logo/fe_svg/fe_source.svg" width="150px">

Fe is an emerging smart contract language for the Ethereum blockchain.

[![Build Status](https://github.com/ethereum/fe/workflows/CI/badge.svg)](https://github.com/ethereum/fe/actions)
[![Coverage](https://codecov.io/gh/ethereum/fe/branch/master/graph/badge.svg)](https://codecov.io/gh/ethereum/fe)

NOTE: **The larger part of the `master` branch will be replaced with the brand-new implementation, which is currently under development in the [fe-v2](https://github.com/ethereum/fe/tree/fe-v2) branch. Please refer to that branch if you plan to contribute to Fe.**

## Overview

Fe is a statically typed language for the Ethereum Virtual Machine (EVM). Inspired by Rust, it is designed to be easy to learn, especially for new developers entering the Ethereum ecosystem.

## Features & Goals

- Bounds and overflow checking
- Decidability by limiting dynamic program behavior
- More precise gas estimation (as a consequence of decidability)
- Static typing
- Pure function support
- Restrictions on reentrancy
- Static looping
- Module imports
- Standard library
- Usage of [YUL](https://docs.soliditylang.org/en/latest/yul.html) IR to target both EVM and eWASM
- WASM compiler binaries for enhanced portability and in-browser compilation of Fe contracts
- Implementation in Rust, a powerful systems-oriented language with strong safety guarantees to reduce the risk of compiler bugs

Additional information about design goals and background can be found in the [official announcement](https://snakecharmers.ethereum.org/fe-a-new-language-for-the-ethereum-ecosystem/).

## Language Specification

We aim to provide a full language specification that should eventually be used to formally verify the correctness of the compiler. A work-in-progress draft of the specification can be found [here](http://fe-lang.org/docs/spec/index.html).

## Progress

Fe development is still in its early stages. We have a basic [Roadmap for 2021](https://notes.ethereum.org/LVhaTF30SJOpkbG1iVw1jg) that we aim to follow. We generally try to drive development by working through real-world use cases. Our next goal is to provide a working Uniswap implementation in Fe, which will help us to advance and shape the language.

Fe had its first alpha release in January 2021 and is now following a monthly release cycle.

## Getting Started

- [Build the compiler](https://github.com/ethereum/fe/blob/master/docs/src/development/build.md)
- [Or download the binary release](https://github.com/ethereum/fe/releases)

To compile Fe code:

1. Run `fe path/to/fe_source.fe`
2. Fe creates a directory `output` in the current working directory that contains the compiled binary and ABI.

Run `fe --help` to explore further options.

## Examples

The following is a simple contract implemented in Fe.

```fe
struct Signed {
    pub book_msg: String<100>
}

contract GuestBook {
    messages: Map<address, String<100>>

    pub fn sign(mut self, mut ctx: Context, book_msg: String<100>) {
        self.messages[ctx.msg_sender()] = book_msg
        ctx.emit(Signed { book_msg })
    }

    pub fn get_msg(self, addr: address) -> String<100> {
        return self.messages[addr].to_mem()
    }
}
