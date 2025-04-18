**问题:** 消耗 1 个普通材料 ($B=1$)，产出 1 个目标物品。有 $Q\%$ 概率回收消耗的材料。初始有 $A$ 个普通材料，求期望产出多少目标物品 $E(A)$？

**核心机制:** "回收" 意味着单次合成可能不消耗材料（净消耗为0），使得理论上可以无限次尝试合成，只要回收成功。

**推导方法 1: 期望递推**

*   **思路:** 每次合成先得 1 个。若回收成功 (概率 $Q$)，状态不变，后续期望还是 $E(A)$；若失败 (概率 $1-Q$)，状态变 $A-1$，后续期望 $E(A-1)$。
*   **公式:**
    $$E(A) = 1 + Q \cdot E(A) + (1-Q) \cdot E(A-1)$$
*   **关键简化/求解思路:** 将 $E(A)$ 项移到左边
    $$(1-Q)E(A) = 1 + (1-Q)E(A-1)$$
    两边同除 $(1-Q)$ 得到
    $$E(A) = \frac{1}{1-Q} + E(A-1)$$
    这是一个等差数列的递推形式（公差为 $\frac{1}{1-Q}$）。结合 $E(0)=0$，可解得：
*   **结果:**
    $$E(A) = \frac{A}{1-Q}$$

**推导方法 2: 几何级数**

*   **思路:** 考虑 **单个** 普通材料的贡献。
    *   第 1 次合成: 必得 1 个 (概率 1)。
    *   第 2 次合成 (需第 1 次回收成功): 贡献 1 个 (概率 $Q$)。
    *   第 3 次合成 (需前 2 次回收成功): 贡献 1 个 (概率 $Q^2$)。
    *   ... 以此类推。
*   **单个材料期望:**
    $$1 + Q \cdot 1 + Q^2 \cdot 1 + Q^3 \cdot 1 + \cdots$$
*   **求和:** 这是首项为 1，公比为 $Q$ 的无限等比级数，和为 $\frac{1}{1-Q}$。
*   **总期望:** $A$ 个材料独立贡献，总期望为 $A \cdot \frac{1}{1-Q}$。
*   **结果:**
    $$E(A) = \frac{A}{1-Q}$$

**最终结论:**

期望产出 $E(A)$ 的计算公式为：
$$\boxed{E(A) = \frac{A}{1 - \frac{Q}{100}}}$$
*(注意：公式中的 $Q$ 代表概率值，计算时使用 $Q/100$ 将百分比转换为小数)*

