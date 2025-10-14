---
title: "解析 EVM Custom Error"
date: 2025-10-14
tags: ["CryptoCurrency", "Ethereum", "EVM", "Smart Contracts", "Solidity", "Blockchain", "Security"]
layout: post
---

# 背景

Solidity 0.8.4 引入的 Custom Error 机制相比传统的 `require(condition, "error message")` 更节省 gas。
当你看到一串十六进制数据时，如何解码出真实的错误信息？
假设我们收到这样一段 revert data：
```
0x6e8698f20000000000000000000000009c5d7cfc31374100b03c0aebe82d5c68df901111
```
`0x6e8698f2` 是 Error Selector，`0000000000000000000000009c5d7cfc31374100b03c0aebe82d5c68df901111` 是 Error Data。

# Custom Error 
## Error Selector 的生成
Custom error 的 selector 是通过对错误签名进行 Keccak-256 哈希，然后取前 4 个字节生成的：

```python
from web3 import Web3

# 错误签名格式：ErrorName(type1,type2,...)
signature = "InsufficientFee(uint256,uint256)"
selector = Web3.keccak(text=signature)[:4].hex()
# 结果: 0xa458261b
```

## ABI 编码规则

错误参数使用 ABI 编码：
- **静态类型**（`uint256`, `address`, `bool` 等）：直接编码为 32 字节
- **动态类型**（`string`, `bytes`, 数组等）：使用偏移量指针 + 长度 + 数据

## selector 生成

```python
def load_abi(abi_path: Path):
    with abi_path.open("r", encoding="utf-8") as abi_file:
        return json.load(abi_file)


def error_signature(error_entry: dict) -> str:
    input_types = ",".join(argument["type"] for argument in error_entry.get("inputs", []))
    return f"{error_entry['name']}({input_types})"


def print_error_selectors(abi_entries: list[dict]) -> None:
    for entry in abi_entries:
        if entry.get("type") != "error":
            continue

        signature = error_signature(entry)
        selector = Web3.keccak(text=signature)[:4].hex()
        print(f"{signature} -> {selector}")


def main() -> None:
    abi_path = Path(__file__).with_name("abi.abi")
    abi_entries = load_abi(abi_path)
    print_error_selectors(abi_entries)


if __name__ == "__main__":
    main()
```

所以，这个错误，其实就是TokenNotTradable(0x9c5d7cfc31374100b03c0aebe82d5c68df901111)
