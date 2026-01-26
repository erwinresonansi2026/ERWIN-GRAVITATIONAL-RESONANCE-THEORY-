#!/usr/bin/env python3
"""
=====================================================
UNIVERSAL RESONANT GRAVITY ENGINE (URGE)
Based on Resonansi Erwin 2026

Refactored:
- Primary phase unit uses π/5 (standard physics notation)
- K36 retained only as an alias (36° sector)
=====================================================
"""

import numpy as np

# =========================
# 1. STRUCTURAL CONSTANTS
# =========================

PI = np.pi

PHASE_UNIT = PI / 5        # Fundamental phase quantum (π/5 radians)
K36 = PHASE_UNIT           # Alias: 36° angular sector (notation only)

# =========================
# 2. BASIC DEFINITIONS
# =========================

def phase(n: int) -> float:
    """
    Discrete gravitational phase
    θ_n = n · (π/5)
    """
    return n * PHASE_UNIT


def resonant_radius(r0: float, alpha: float, n: int) -> float:
    """
    Universal resonant orbit formula

    r_n = r0 · exp(α · n · (π/5))
    """
    return r0 * np.exp(alpha * phase(n))


# =========================
# 3. PARAMETER INFERENCE
# =========================

def infer_alpha(radii: list[float]) -> float:
    """
    Infer resonance scaling parameter α
    from ordered orbital radii
    """
    radii = np.array(radii, dtype=float)

    if len(radii) < 2:
        raise ValueError("At least two radii required")

    ratios = np.log(radii[1:] / radii[:-1])
    return np.mean(ratios) / PHASE_UNIT


# =========================
# 4. GENERIC RESONANT SOLVER
# =========================

def generate_resonant_system(
    observed_radii: list[float],
    labels: list[str] | None = None
):
    """
    Generic resonance-based gravitational system solver
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
            "phase_rad": phase(i),
            "phase_unit_multiple": i
        })

    return alpha, results


# =========================
# 5. EXAMPLE USAGE
# =========================

if __name__ == "__main__":

    observed = [
        0.387, 0.723, 1.000, 1.524,
        5.203, 9.537, 19.191, 30.068
    ]

    labels = [
        "Mercury", "Venus", "Earth", "Mars",
        "Jupiter", "Saturn", "Uranus", "Neptune"
    ]

    alpha, system = generate_resonant_system(observed, labels)

    print("\n=== UNIVERSAL RESONANT SOLUTION (π/5) ===")
    print(f"Phase unit (π/5) = {PHASE_UNIT:.6f} rad")
    print(f"alpha           = {alpha:.6f}\n")

    for obj in system:
        print(
            f"{obj['label']:<8} | "
            f"Obs={obj['observed']:.3f} | "
            f"Mod={obj['model']:.3f} | "
            f"Err={obj['error_percent']:+.2f}% | "
            f"Phase={obj['phase_rad']:.3f} rad"
        )
