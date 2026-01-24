#!/usr/bin/env python3
"""
=====================================================
UNIVERSAL RESONANT GRAVITY ENGINE (URGE)
Based on Resonansi Erwin 2026

Purpose:
- Compute resonant orbital structure
- Works for ANY gravitational system
- Explicit about required variables
- No hidden assumptions
=====================================================
"""

import numpy as np

# =========================
# 1. KONSTANTA STRUKTURAL
# =========================

PI = np.pi
K36 = PI / 5        # Universal phase quantum (36 degrees)

# =========================
# 2. DEFINISI DASAR
# =========================

def phase(n: int) -> float:
    """
    Discrete gravitational phase
    """
    return n * K36


def resonant_radius(r0: float, alpha: float, n: int) -> float:
    """
    Universal resonant orbit formula

    r_n = r0 * exp(alpha * n * K36)
    """
    return r0 * np.exp(alpha * phase(n))


# =========================
# 3. FITTING DARI DATA MINIMUM
# =========================

def infer_alpha(radii: list[float]) -> float:
    """
    Infer resonance scaling parameter alpha
    from ANY ordered orbital data
    """
    radii = np.array(radii)
    if len(radii) < 2:
        raise ValueError("At least two radii required")

    ratios = np.log(radii[1:] / radii[:-1])
    return np.mean(ratios) / K36


# =========================
# 4. GENERATOR SISTEM SEMESTA
# =========================

def generate_resonant_system(
    observed_radii: list[float],
    labels: list[str] | None = None
):
    """
    Generic universe-scale resonant solver
    """

    if labels is None:
        labels = [f"Object-{i}" for i in range(len(observed_radii))]

    r0 = observed_radii[0]
    alpha = infer_alpha(observed_radii)

    results = []

    for i, label in enumerate(labels):
        r_model = resonant_radius(r0, alpha, i)
        r_obs = observed_radii[i]
        error = (r_model - r_obs) / r_obs * 100

        results.append({
            "label": label,
            "observed": r_obs,
            "model": r_model,
            "error_percent": error,
            "phase": phase(i)
        })

    return alpha, results


# =========================
# 5. CONTOH PEMAKAIAN
# =========================

if __name__ == "__main__":
    # Contoh: bisa Tata Surya, satelit, atau sistem lain
    observed = [
        0.387, 0.723, 1.0, 1.524,
        5.203, 9.537, 19.191, 30.068
    ]

    labels = [
        "Mercury", "Venus", "Earth", "Mars",
        "Jupiter", "Saturn", "Uranus", "Neptune"
    ]

    alpha, system = generate_resonant_system(observed, labels)

    print("\n=== UNIVERSAL RESONANT SOLUTION ===")
    print(f"K36   = {K36:.6f}")
    print(f"alpha= {alpha:.6f}\n")

    for obj in system:
        print(
            f"{obj['label']:<8} | "
            f"Obs={obj['observed']:.3f} | "
            f"Mod={obj['model']:.3f} | "
            f"Err={obj['error_percent']:+.2f}% | "
            f"Phase={obj['phase']:.3f}"
        )
