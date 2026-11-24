# Reliability-Analysis
Conducted reliability assessment of a complex missile system using FMECA-based reliability allocation, component failure-rate prediction, environmental test data analysis, and statistical reliability demonstration for continuous and one-shot devices.

#oneshotdevicenumberof trials need
from scipy.stats import beta


prior_a = float(input("Enter prior a (successes): "))
prior_b = float(input("Enter prior b (failures): "))

target_R = float(input("Enter target reliability R: "))
confidence = float(input("Enter confidence level (e.g. 0.90): "))

new_failures = int(
    input("Enter failures allowed in new tests: ")
)


if prior_a <= 0:
    prior_a = 0.5

if prior_b <= 0:
    prior_b = 0.5


p_target = 1 - target_R


N = new_failures

max_N = 1000000

while N <= max_N:

    new_successes = N - new_failures

    a_post = prior_a + new_successes
    b_post = prior_b + new_failures

    p_upper = beta.ppf(
        confidence,
        b_post,
        a_post
    )


    if p_upper <= p_target:
        break

    N += 1

else:
    raise ValueError(
        "Required sample size exceeds the maximum allowed limit."
    )



E_R = a_post / (a_post + b_post)


print("\n--- Results ---")

print("Required number of NEW test samples (N) =", N)

print("Posterior expected reliability E[R] =", float(E_R))

print("Target reliability =", target_R)

print("Confidence level =", confidence)
