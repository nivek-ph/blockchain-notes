# 比特币开发文档

欢迎来到比特币开发文档！这里包含了完整的比特币开发指南，从基础概念到高级应用。

## 📚 文档结构

### 1. 地址管理
- **[地址生成](./address/address.md)** - Base58和Bech32地址生成
- **[传统地址](./address/legacy.md)** - P2PKH和P2SH地址格式
- **[多重签名地址](./address/multisig.md)** - 多签地址创建和管理

### 2. 脚本系统
- **[脚本基础](./script/script.md)** - 脚本编译、反编译和操作
- **[脚本分类](./script/classify.md)** - 标准脚本类型识别

### 3. 交易处理
- **[比特币交易](./tx/bitcoin.md)** - PSBT交易构建和签名
- **[BitGo集成](./tx/bitgo.md)** - 企业级钱包集成
- **[交易解码](./tx/decode.md)** - 交易数据解析
- **[RBF替换](./tx/rbf.md)** - 费用替换技术

### 4. 工具函数
- **[加密函数](./utils/crypto.md)** - SHA256、RIPEMD160等哈希函数

## 🚀 快速开始

### 环境准备
```bash
# 安装依赖
npm install bitcoinjs-lib bech32 bs58

# 或者使用yarn
yarn add bitcoinjs-lib bech32 bs58
```

### 生成地址
```javascript
const { ECPair, payments, networks } = require('bitcoinjs-lib');

// 创建密钥对
const keyPair = ECPair.makeRandom({ network: networks.testnet });

// 生成P2PKH地址
const { address } = payments.p2pkh({ 
  pubkey: keyPair.publicKey, 
  network: networks.testnet 
});

console.log('Address:', address);
console.log('Private Key:', keyPair.privateKey.toString('hex'));
```

### 构建交易
```javascript
const { Psbt } = require('bitcoinjs-lib');

// 创建PSBT
const psbt = new Psbt({ network: networks.testnet });

// 添加输入
psbt.addInput({
  hash: 'previous_transaction_hash',
  index: 0,
  nonWitnessUtxo: Buffer.from('previous_tx_hex', 'hex')
});

// 添加输出
psbt.addOutput({
  address: 'recipient_address',
  value: 50000 // 50000 satoshis
});

// 签名
psbt.signInput(0, keyPair);
psbt.finalizeAllInputs();

// 获取交易
const transaction = psbt.extractTransaction();
console.log('Transaction:', transaction.toHex());
```

## 🔧 开发工具

- **bitcoinjs-lib** - 比特币JavaScript库
- **bitcoin-ops** - 操作码定义
- **bech32** - Bech32编码库  
- **bs58** - Base58编码库
- **tiny-secp256k1** - 椭圆曲线库

## 🌐 网络环境

| 网络     | 地址前缀 | Bech32前缀 | 链ID | 状态     |
| -------- | -------- | ---------- | ---- | -------- |
| 主网     | 1, 3     | bc1        | -    | ✅ 活跃   |
| 测试网   | m, n, 2  | tb1        | -    | ✅ 活跃   |
| 回归测试 | m, n, 2  | bcrt1      | -    | 🔧 可配置 |

## 📖 核心概念

### UTXO模型
- **Unspent Transaction Output** - 未花费交易输出
- **Input** - 交易输入，引用前一个UTXO
- **Output** - 交易输出，创建新的UTXO
- **找零** - 多余的资金返回给发送者

### 地址格式
- **Legacy (P2PKH)** - 以"1"开头，传统格式
- **P2SH** - 以"3"开头，脚本哈希格式
- **Native SegWit (P2WPKH)** - 以"bc1"开头，见证格式
- **Taproot (P2TR)** - 以"bc1p"开头，最新格式

### 脚本系统
- **锁定脚本** - 定义花费条件
- **解锁脚本** - 提供花费证明
- **栈执行** - 基于栈的脚本执行
- **操作码** - 脚本指令集

## 📖 学习路径

### 初学者
1. 了解比特币基础概念
2. 学习UTXO模型
3. 掌握地址生成和验证

### 进阶开发者  
1. 深入理解脚本系统
2. 学习PSBT交易构建
3. 掌握多重签名技术

### 高级开发者
1. 构建Layer 2解决方案
2. 优化交易费用和大小
3. 实现复杂的智能合约

## 🔐 安全最佳实践

1. **私钥保护** - 永远不要暴露私钥
2. **网络选择** - 开发时使用测试网络
3. **费用设置** - 根据网络状况设置合理费用
4. **交易验证** - 在广播前验证所有交易
5. **备份恢复** - 实现可靠的密钥备份机制

## 🤝 贡献指南

欢迎贡献代码和文档！请遵循以下步骤：

1. Fork 本仓库
2. 创建特性分支
3. 提交更改
4. 推送到分支
5. 创建 Pull Request

## 📞 支持

如果你遇到问题或需要帮助：

- 查看 [常见问题](./FAQ.md)
- 提交 [Issue](https://github.com/nivek-phx/blockchain-notes/issues)
- 加入 [Telegram](https://t.me/bitcoin_dev)

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](https://github.com/nivek-phx/blockchain-notes/blob/main/LICENSE) 文件了解详情。

## 🔗 相关资源

- [Bitcoin Core](https://bitcoincore.org/) - 比特币核心客户端
- [Bitcoin Developer Guide](https://developer.bitcoin.org/) - 官方开发指南
- [Mastering Bitcoin](https://github.com/bitcoinbook/bitcoinbook) - 精通比特币
- [Bitcoin Improvement Proposals](https://github.com/bitcoin/bips) - BIP提案

---

**开始你的比特币开发之旅吧！** ⚡
