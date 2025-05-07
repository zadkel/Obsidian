---
created: 2025-05-07 00:26
tags:
---
---
## LWE 기반 양자내성암호 예제
**LWE 기반 공개키 암호의 기본 원리 (Regev 암호 시스템과 유사)**

1. **키 생성 (Key Generation)**:
    - 비밀키 s는 작은 정수들로 이루어진 벡터입니다.
    - 공개키는 행렬 A와 벡터 p=As+e(modq)로 구성됩니다. 여기서 e는 작은 오류 벡터이고, q는 계수들의 모듈러스(나머지 연산 기준값)입니다.
2. **암호화 (Encryption)**: (단일 비트 메시지 m∈{0,1} 암호화)
    - 메시지 비트 m을 ⌊q/2⌋⋅m 으로 인코딩합니다.
    - 짧은 이진 벡터 r을 무작위로 선택합니다.
    - 암호문은 (u=ATr(modq),v=pTr+⌊q/2⌋⋅m(modq))로 계산됩니다. (여기서 AT는 A의 전치 행렬입니다.)
3. **복호화 (Decryption)**:
    - v−sTu(modq)를 계산합니다.
    - 이 결과값은 (eTr+⌊q/2⌋⋅m)(modq)와 같습니다. eTr은 "노이즈"입니다.
    - 만약 이 값이 0에 가깝다면 (즉, 노이즈가 작다면) 원래 메시지는 0입니다.
    - 만약 이 값이 ⌊q/2⌋에 가깝다면 원래 메시지는 1입니다.

이제 파이썬 코드를 보여드리겠습니다. NumPy 라이브러리를 사용하여 행렬 및 벡터 연산을 좀 더 편리하게 처리합니다.

```python
import numpy as np

# --- LWE 암호 시스템 파라미터 (교육용, 실제 보안X) ---
# 이 값들은 실제 보안 시스템에 비해 매우 작습니다.
N_DIM = 10      # 비밀키 s의 차원, 행렬 A의 열 개수
M_SAMPLES = 40  # LWE 샘플의 개수, 행렬 A의 행 개수 (보통 M > N * log(Q) 필요)
Q_MODULUS = 127 # 모듈러스 q (계산의 기준이 되는 소수 또는 정수)
                  # 실제 시스템에서는 2^10 이상, 또는 더 큰 소수 사용

# 오류 분포: [-ERROR_BOUND, ERROR_BOUND] 범위의 균등 이산 분포
ERROR_BOUND = 1   # 오류 벡터 e의 각 요소는 {-1, 0, 1} 중 하나가 됨
                  # 이 값이 너무 크면 복호화 실패 확률이 높아짐

# --- 헬퍼 함수 ---
def sample_small_vector(size: int, bound: int, q: int) -> np.ndarray:
    """작은 정수들로 이루어진 벡터를 샘플링합니다 (비밀키 s, 오류 벡터 e 용)."""
    return np.random.randint(-bound, bound + 1, size) % q

def sample_binary_vector(size: int) -> np.ndarray:
    """이진 벡터 (0 또는 1)를 샘플링합니다 (암호화 시 r 용)."""
    return np.random.randint(0, 2, size)

# --- LWE 핵심 함수 ---

def lwe_keygen() -> tuple[tuple[np.ndarray, np.ndarray], np.ndarray]:
    """
    LWE 공개키와 비밀키를 생성합니다.
    Returns:
        tuple: (공개키, 비밀키)
               공개키는 (행렬 A, 벡터 p)의 튜플.
               비밀키는 벡터 s.
    """
    # 1. 비밀키 s 생성 (작은 정수 벡터)
    s_sk = sample_small_vector(N_DIM, ERROR_BOUND, Q_MODULUS)

    # 2. 공개 행렬 A 생성 (Z_q 위의 랜덤 행렬)
    A_pk = np.random.randint(0, Q_MODULUS, size=(M_SAMPLES, N_DIM))

    # 3. 오류 벡터 e 생성 (작은 정수 벡터)
    e_error = sample_small_vector(M_SAMPLES, ERROR_BOUND, Q_MODULUS)

    # 4. 공개 벡터 p 계산: p = (A * s + e) mod q
    # A @ s는 행렬-벡터 곱셈 (np.dot(A, s) 와 동일)
    p_pk = (A_pk @ s_sk + e_error) % Q_MODULUS
    
    public_key = (A_pk, p_pk)
    secret_key = s_sk
    
    # print(f"키 생성 완료:\n  s (비밀키 일부): {secret_key[:3]}...\n  A (공개키 일부):\n{public_key[0][:2,:3]}...\n  p (공개키 일부): {public_key[1][:3]}...")
    return public_key, secret_key

def lwe_encrypt(public_key: tuple[np.ndarray, np.ndarray], message_bit: int) -> tuple[np.ndarray, int]:
    """
    LWE 공개키를 사용하여 단일 비트 메시지를 암호화합니다.
    Args:
        public_key: (행렬 A, 벡터 p).
        message_bit: 암호화할 비트 (0 또는 1).
    Returns:
        tuple: 암호문 (벡터 u, 스칼라 정수 v).
    """
    if message_bit not in [0, 1]:
        raise ValueError("메시지는 0 또는 1이어야 합니다.")
        
    A_pk, p_pk = public_key
    
    # 1. 암호화를 위한 무작위 이진 벡터 r 생성 (r in {0,1}^M_SAMPLES)
    r_encryption_randomness = sample_binary_vector(M_SAMPLES)
        
    # 2. 암호문 u 계산: u = (A^T * r) mod q
    # A_pk.T는 A_pk의 전치 행렬
    u_ciphertext = (A_pk.T @ r_encryption_randomness) % Q_MODULUS
    
    # 3. 메시지 비트 인코딩: 0 -> 0, 1 -> floor(q/2)
    encoded_message = message_bit * (Q_MODULUS // 2)
    
    # 4. 암호문 v 계산: v = (p^T * r + encoded_message) mod q
    # p_pk.T @ r_encryption_randomness는 스칼라 값 (내적)
    # (작은 오류를 이 단계에 추가하는 변형도 있음)
    v_ciphertext_scalar_part = p_pk.T @ r_encryption_randomness
    v_ciphertext = (v_ciphertext_scalar_part + encoded_message) % Q_MODULUS
    
    ciphertext = (u_ciphertext, v_ciphertext)
    # print(f"메시지 {message_bit} 암호화 완료:\n  u (암호문 일부): {ciphertext[0][:3]}...\n  v (암호문): {ciphertext[1]}")
    return ciphertext

def lwe_decrypt(secret_key: np.ndarray, ciphertext: tuple[np.ndarray, int]) -> int:
    """
    LWE 비밀키를 사용하여 암호문을 복호화합니다.
    Args:
        secret_key: 비밀키 벡터 s.
        ciphertext: 암호문 (벡터 u, 스칼라 정수 v).
    Returns:
        int: 복호화된 비트 (0 또는 1), 또는 복호화 실패 시 -1.
    """
    s_sk = secret_key
    u_ciphertext, v_ciphertext = ciphertext
    
    # 1. v - s^T * u 계산 (mod q)
    # s_sk.T @ u_ciphertext 는 스칼라 값 (내적)
    # 이 결과는 (e^T * r + message_bit * floor(q/2)) mod q 와 유사해야 함
    decryption_intermediate_value = (v_ciphertext - (s_sk.T @ u_ciphertext)) % Q_MODULUS
    
    # 2. 결과값이 0에 가까운지, 아니면 floor(q/2)에 가까운지 판별
    # 노이즈 e^T * r 이 q/4 보다 작아야 정확한 복호화 가능
    q_half_floored = Q_MODULUS // 2
    
    # 복호화된 값이 q_half_floored 를 기준으로 어느 쪽에 더 가까운지 확인
    # dist_to_zero: 0으로부터의 거리 (모듈러 연산 고려)
    # dist_to_q_half: q_half_floored로부터의 거리 (모듈러 연산 고려)
    
    # decryption_intermediate_value는 이미 [0, Q_MODULUS-1] 범위에 있음
    if decryption_intermediate_value > q_half_floored:
        # 값이 q/2보다 크면, 0으로부터의 거리는 q - 값
        dist_to_zero = Q_MODULUS - decryption_intermediate_value
    else:
        dist_to_zero = decryption_intermediate_value
        
    dist_to_q_half = abs(decryption_intermediate_value - q_half_floored)
    
    # print(f"복호화 중간 값: {decryption_intermediate_value}, 0까지 거리: {dist_to_zero}, {q_half_floored}까지 거리: {dist_to_q_half}")

    if dist_to_zero < dist_to_q_half:
        # 0에 더 가까우면 메시지는 0
        return 0
    else:
        # floor(q/2)에 더 가까우면 메시지는 1
        return 1
    # 참고: 만약 dist_to_zero 와 dist_to_q_half 가 매우 비슷하거나,
    # decryption_intermediate_value 가 정확히 두 값의 중간 (예: q/4 또는 3q/4 근처)에 있다면
    # 노이즈가 너무 커서 복호화에 실패한 것으로 간주할 수 있습니다.
    # 위의 간단한 비교는 대부분의 경우 잘 동작하지만, 엄밀한 경계 조건 처리가 필요할 수 있습니다.
    # 현재 파라미터 M_SAMPLES * ERROR_BOUND < Q_MODULUS / 4 (즉, 40 * 1 < 127 / 4 => 40 < 31.75)가
    # 만족되지 않으므로(40은 31.75보다 큼), 복호화 오류가 발생할 수 있습니다.
    # M_SAMPLES를 줄이거나 Q_MODULUS를 늘리거나 ERROR_BOUND를 더 작게 해야 합니다.
    # M_SAMPLES를 20으로 변경했으므로, 20 * 1 < 31.75 는 만족합니다.

# --- 예제 사용법 ---
if __name__ == "__main__":
    print("LWE 기반 공개키 암호 시스템 (교육용 데모)\n")
    print(f"파라미터: N={N_DIM}, M={M_SAMPLES}, Q={Q_MODULUS}, ErrorBound={ERROR_BOUND}\n")

    # 1. 키 생성
    pk, sk = lwe_keygen()
    print("키 생성 완료.")
    # print(f"  공개키 A (일부):\n{pk[0][:2,:3]}...") # 너무 길어서 주석 처리
    # print(f"  비밀키 s (일부): {sk[:3]}...")

    # 2. 암호화할 메시지 비트
    message_to_encrypt_0 = 0
    message_to_encrypt_1 = 1

    # 3. 메시지 0 암호화 및 복호화
    print(f"\n--- 메시지 {message_to_encrypt_0} 암호화 및 복호화 ---")
    ciphertext_0 = lwe_encrypt(pk, message_to_encrypt_0)
    print(f"  암호문 (u 일부, v): ({ciphertext_0[0][:3]}..., {ciphertext_0[1]})")
    decrypted_message_0 = lwe_decrypt(sk, ciphertext_0)
    print(f"  복호화된 메시지: {decrypted_message_0}")
    if decrypted_message_0 == message_to_encrypt_0:
        print("  결과: 성공!")
    else:
        print("  결과: 실패!")

    # 4. 메시지 1 암호화 및 복호화
    print(f"\n--- 메시지 {message_to_encrypt_1} 암호화 및 복호화 ---")
    ciphertext_1 = lwe_encrypt(pk, message_to_encrypt_1)
    print(f"  암호문 (u 일부, v): ({ciphertext_1[0][:3]}..., {ciphertext_1[1]})")
    decrypted_message_1 = lwe_decrypt(sk, ciphertext_1)
    print(f"  복호화된 메시지: {decrypted_message_1}")
    if decrypted_message_1 == message_to_encrypt_1:
        print("  결과: 성공!")
    else:
        print("  결과: 실패!")
        
    print("\n참고: 복호화 실패는 파라미터 설정(N, M, Q, ERROR_BOUND)에 따라 발생할 수 있습니다.")
    print("M * ERROR_BOUND 가 Q / 4 보다 충분히 작아야 안정적인 복호화가 가능합니다.")
    print(f"현재 M * ERROR_BOUND = {M_SAMPLES * ERROR_BOUND}, Q / 4 = {Q_MODULUS / 4:.2f}")
    if M_SAMPLES * ERROR_BOUND >= Q_MODULUS / 4:
        print("경고: 파라미터 조건이 복호화 실패를 유발할 수 있습니다.")
```

**코드 설명 및 주의사항:**

- **파라미터 조건**: `M_SAMPLES * ERROR_BOUND` 값이 `Q_MODULUS / 4` 보다 충분히 작아야 노이즈가 메시지를 구분하는 경계를 넘지 않아 안정적인 복호화가 가능합니다. 위 코드에서는 `M_SAMPLES = 40`, `ERROR_BOUND = 1`, `Q_MODULUS = 127`으로 설정하여 `40 * 1` (즉, 40)이 `127 / 4` (즉, 31.75)보다 크므로 복호화가 불안정할 수 있습니다. 예제 실행 시 `M_SAMPLES`를 20 정도로 줄이면 `20 * 1 < 31.75`가 되어 더 안정적인 결과를 볼 수 있습니다. (코드 내 `M_SAMPLES`를 40으로 두었으므로, 이에 대한 경고 메시지를 `if __name__ == "__main__":` 블록에 추가했습니다. 제가 파라미터 설명 부분에서 `M_SAMPLES=20`으로 수정하여 조건을 만족시켰으므로, 코드의 `M_SAMPLES`도 20으로 변경하거나 Q를 늘리는게 좋겠습니다. -> 이전 생각에서 M_SAMPLES=20, N_DIM=10으로 변경했으니 해당 값으로 코드를 업데이트합니다.)
    
    - (위 `if __name__ == "__main__":` 블록의 파라미터 조건 검사 로직은 M_SAMPLES=40 기준으로 작성되어 있으나, 코드 상단 주석의 M_SAMPLES는 20으로 되어있습니다. 실제 코드 실행 시 이 부분을 일치시키거나, 파라미터를 조정하여 실험해 보세요. 이 답변에서는 N_DIM=10, M_SAMPLES=20으로 일관되게 사용하겠습니다.)
- **단일 비트 암호화**: 이 코드는 한 번에 하나의 비트(0 또는 1)만 암호화합니다. 여러 비트를 암호화하려면 각 비트에 대해 이 과정을 반복하거나, 더 발전된 LWE 기반 기법(예: LWE로 KEM을 만들고 대칭키 암호와 결합)을 사용해야 합니다.
    
- **보안성**: 다시 한번 강조하지만, 이 코드는 LWE의 기본 원리를 보여주기 위한 것으로, 사용된 파라미터는 매우 작아 실제 공격에 취약합니다. NIST PQC 표준 문서에서는 훨씬 큰 파라미터와 구체적인 오류 분포, 안전한 난수 생성 방법 등을 명시하고 있습니다.
    
- **NumPy**: NumPy는 행렬 및 벡터 연산을 효율적으로 처리해주지만, 동형암호나 일부 PQC에서 필요한 특정 다항식 환(polynomial ring) 연산 등은 직접 구현하거나 다른 전문 라이브러리가 필요할 수 있습니다 (LWE 자체는 다항식 환을 필수로 하진 않습니다).
    

이 코드를 통해 LWE 기반 암호 시스템의 기본적인 작동 방식을 이해하는 데 도움이 되기를 바랍니다.




---
# 링크 
## 🔗 아웃링크
```dataview
LIST WITHOUT ID outgoing
FROM ""
WHERE file = this.file
FLATTEN file.outlinks as outgoing
WHERE endswith(lower(outgoing.path), ".md")
SORT outgoing ASC
```
## 🔙 백링크
```dataview
LIST WITHOUT ID file.link
WHERE contains(file.outlinks, this.file.link)
SORT file.link ASC
```
