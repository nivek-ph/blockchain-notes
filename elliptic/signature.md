### ECDSA vs EdDSA

椭圆曲线数字签名算法的两种主要实现方式对比。

## 基本概念

### ECDSA (Elliptic Curve Digital Signature Algorithm)
- **标准**: FIPS 186-4, ANSI X9.62
- **曲线**: secp256k1, secp256r1 (P-256), secp384r1 等
- **特点**: 传统标准，广泛采用，需要随机数生成器

### EdDSA (Edwards-Curve Digital Signature Algorithm)
- **标准**: RFC 8032
- **曲线**: Ed25519 (Edwards25519), Ed448
- **特点**: 现代设计，确定性签名，性能更优

## 技术对比

| 特性           | ECDSA                               | EdDSA                    |
| -------------- | ----------------------------------- | ------------------------ |
| **签名算法**   | 非确定性（需要随机数）              | 确定性（基于私钥和消息） |
| **随机数要求** | 必须使用安全的随机数生成器          | 不需要外部随机数         |
| **签名大小**   | 64-72 字节（secp256k1: 64字节）     | 64 字节（Ed25519）       |
| **公钥大小**   | 33 字节（压缩）或 65 字节（未压缩） | 32 字节（Ed25519）       |
| **私钥大小**   | 32 字节                             | 32 字节（Ed25519）       |
| **签名速度**   | 较慢                                | 更快（约快 2-3 倍）      |
| **验证速度**   | 较慢                                | 更快（约快 2-3 倍）      |
| **密钥生成**   | 需要随机数                          | 确定性（从种子派生）     |

## 安全性对比

### ECDSA 的安全考虑
- ⚠️ **随机数重用风险**: 如果随机数 k 被重用或可预测，私钥可能泄露
- ⚠️ **侧信道攻击**: 需要防护时序攻击
- ✅ **成熟稳定**: 经过长期验证，广泛部署

### EdDSA 的安全优势
- ✅ **无随机数风险**: 确定性签名消除了随机数相关的漏洞
- ✅ **侧信道防护**: 设计时考虑了侧信道攻击防护
- ✅ **更简单的实现**: 减少实现错误的可能性
- ✅ **现代密码学**: 基于最新的密码学研究

## 性能对比

### 签名性能
- **EdDSA**: ~50,000 签名/秒（Ed25519）
- **ECDSA**: ~20,000 签名/秒（secp256k1）

### 验证性能
- **EdDSA**: ~60,000 验证/秒（Ed25519）
- **ECDSA**: ~25,000 验证/秒（secp256k1）

## 应用场景

### ECDSA 主要应用
- **Bitcoin**: secp256k1
- **Ethereum**: secp256k1
- **传统金融系统**: secp256r1 (P-256)
- **TLS/SSL**: 广泛支持

### EdDSA 主要应用
- **Solana**: Ed25519
- **Cardano**: Ed25519
- **Zcash**: Ed25519（部分场景）
- **现代区块链项目**: 新项目倾向于使用 Ed25519
- **SSH**: Ed25519 密钥
- **TLS 1.3**: 支持 Ed25519

## 代码实现对比

### ECDSA (secp256k1)
```typescript
// 需要随机数生成器
import { ec as EC } from 'elliptic';
const ec = new EC('secp256k1');

const keyPair = ec.genKeyPair();
const msgHash = hash(message);
const signature = keyPair.sign(msgHash); // 内部使用随机数 k
```

### EdDSA (Ed25519)
```typescript
// 确定性签名，无需随机数
import { ed25519 } from '@noble/curves/ed25519';

const privateKey = randomBytes(32);
const publicKey = ed25519.getPublicKey(privateKey);
const signature = ed25519.sign(message, privateKey); // 确定性
```

## 选择建议

### 选择 ECDSA 如果：
- 需要与现有系统（如 Bitcoin、Ethereum）兼容
- 遵循传统标准和规范
- 已有成熟的 ECDSA 基础设施

### 选择 EdDSA 如果：
- 构建新的区块链或系统
- 需要更高的性能和安全性
- 希望简化实现（无需随机数管理）
- 追求更小的密钥和签名大小

## 总结

| 维度           | 推荐        |
| -------------- | ----------- |
| **性能**       | EdDSA ⭐⭐⭐⭐⭐ |
| **安全性**     | EdDSA ⭐⭐⭐⭐⭐ |
| **兼容性**     | ECDSA ⭐⭐⭐⭐⭐ |
| **实现复杂度** | EdDSA ⭐⭐⭐⭐⭐ |
| **标准化程度** | ECDSA ⭐⭐⭐⭐⭐ |

**结论**: 对于新项目，EdDSA (Ed25519) 是更好的选择，提供了更好的性能、安全性和实现简单性。对于需要与现有系统兼容的场景，ECDSA 仍然是必要的选择。

## 参考使用

- **EdDSA**: 见 [ed25519.ts](./ed25519.ts)
- **ECDSA**: 见 [secp256k1.ts](./secp256k1.ts)