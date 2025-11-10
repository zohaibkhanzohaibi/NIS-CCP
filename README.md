# 🛡️ Cryptanalysis of the Composite Vigenère Cipher

This project presents the **design, implementation, and security analysis** of a custom **Composite Vigenère Cipher**.  
We rigorously evaluated its resistance against both **statistical (Frequency Analysis)** and **algebraic (Known-Plaintext Attack)** methods, identifying critical structural weaknesses and proposing necessary security enhancements.

---

## 🧑‍💻 Project Team

| Name                | Roll Number |
|---------------------|-------------|
| Laibah Shahid       | CT-22061    |
| Tooba Kashaf        | CT-22068    |
| Haiqa Siddiqua      | CT-22071    |
| **Muhammad Zohaib Khan** | **CT-22072** |

---

## 🔒 The Composite Vigenère Cipher

The cipher utilizes a **multi-round, hybrid approach** that iterates a sequence of **Caesar Shift** and **Vigenère** operations, repeated for the full length of the key (**|K|**):

\[
\text{Encryption} = (\text{Vigenère}_K \circ \text{Caesar}_{k_r})^{\times |K|}
\]

This design was intended to maximize **confusion (Vigenère)** and **diffusion (iterative shifts)**.

---

## 📊 Cryptanalysis Findings Summary

Our analysis confirmed that the cipher is fundamentally flawed due to its **linear modular structure**.

### 1. Frequency Analysis (CPA) Results

The statistical attack showed vulnerability is highly dependent on ciphertext length (**L**):

| Attack Method | Key Length m ≥ 10 | Key Length m ≤ 7 |
|----------------|------------------|------------------|
| **Frequency Analysis (CPA)** | Highly Secure (**0% Success**) | Highly Vulnerable (**100% Success for L ≥ 300**) |

---

### 2. Known-Plaintext Attack (KPA) Results

The algebraic attack exposed the ultimate weakness:

| Metric | Case 1: m ∈ {4, 10, 12}, gcd(m, 26) = 2 | Case 2: m = 7, gcd(m, 26) = 1 |
|--------|------------------------------------------|--------------------------------|
| **KPA Success Rate** | 100% (Instantly solved) | 100% (Instantly solved) |
| **Key Space Reduction** | Reduced to ≈ **2^m** possibilities (e.g., 2,048 keys for m=12) | Reduced to a **trivial 2 candidates** |

The KPA demonstrates that the cipher's linear structure allows the **key space to be mathematically decimated**, making the system unfit for secure communication.

---

## 💡 Recommendations for Improvement

To secure the cipher against the discovered linear vulnerabilities, the following structural changes are essential:

1. **Disrupt Linearity** – Introduce **Inter-Columnar Key Mixing** and use **Nonlinear Modular Mapping** to prevent attackers from solving the modular congruences.  
2. **Break Symmetry** – Implement a **Keyed Initialization Vector (IV)** to randomize the aggregate key sum (**S_total**) term, eliminating the critical KPA outer loop.  
3. **Irregular Scheduling** – Employ a **variable or irregular key application schedule** to remove the mathematical repetition factor (**m · k_t**) that causes ambiguity.

---

## 📂 Project File Structure

| File / Directory | Description |
|------------------|-------------|
| `Encrypt_decrypt.ipynb` | Core Cipher Implementation (Encryption and Decryption logic). |
| `Frequency_Attack.ipynb` | Code and analysis for the Frequency Analysis Attack (CPA). |
| `Known_Plaintext_Attack.ipynb` | Code and analysis for the Known-Plaintext Attack (KPA). |
| `README.md` | This documentation file. |
| `frequency_analysis_attack/` | Contains performance data (.csv) and visualizations (.png) for the statistical attack. |
| `known_plaintext_attack/` | Contains performance data (.csv) and visualizations (.png) for the algebraic attack. |

---

### 📈 Example Metric Table

| Metric | Calculation Method | Interpretation |
|--------|--------------------|----------------|
| **Success Rate** | (Count of trials where true key ∈ candidate list) / (Total trials) | Measures the mathematical correctness of the KPA solver. |
| **Average Candidate Count** | Mean count of keys returned by the KPA solver. | Measures the ambiguity or remaining work. |
| **Average Time (s)** | Mean time taken to run the KPA solver. | Measures the attack's computational cost. |

---

## ⚙️ Technologies Used
- Python (NumPy, Pandas, Matplotlib)
- Jupyter Notebook
- Custom Cipher Modules

---

## 📢 Summary
The Composite Vigenère Cipher highlights how **layering weak ciphers doesn’t guarantee strength**.  
Our cryptanalysis demonstrates that even complex hybrid encryption schemes can collapse under **algebraic scrutiny** if linear dependencies are not eliminated.

---

> 🧠 *“Security by obscurity fails when mathematics speaks.”*
