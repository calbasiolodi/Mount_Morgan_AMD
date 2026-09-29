"""
Compute both Pearson (linear) and Spearman (rank/monotonic, more suitable to filter out distortion by outliers) correlations
between pH and Electrical Conductivity variables and all other hydrological variables and temperature for both monitoring location (Dee River at Kenbula
and Dee River at Wura).

Outputs the results into a detailed formatted text file.
"""

import os
import numpy as np
import pandas as pd


def compute_correlations():
    base_dir = r"Mount Morgan Mine"
    input_csv = os.path.join(base_dir, "merged_locations_monthly_stacked.csv")
    output_txt = os.path.join(base_dir, "ph_conductivity_correlations.txt")

    print(f"Reading dataset: {input_csv}")
    df = pd.read_csv(input_csv)

    # Exclude non-numeric and metadata columns
    exclude_cols = {"Station_Code", "Station_Name", "Date_Time", "Date", "Comments"}
    numeric_cols = [c for c in df.columns if c not in exclude_cols]

    # Convert numeric columns explicitly
    for c in numeric_cols:
        df[c] = pd.to_numeric(df[c], errors="coerce")

    # Define target variables (pH and Electrical Conductivity)
    target_all = [c for c in numeric_cols if "pH" in c or "E Conduct." in c]
    target_measurements = [c for c in target_all if not c.endswith("_Quality")]

    # Other environmental variables to correlate against
    other_all = [c for c in numeric_cols if c not in target_all]
    other_measurements = [c for c in other_all if not c.endswith("_Quality")]

    lines = []
    lines.append("=" * 95)
    lines.append("HYDROLOGICAL & WATER QUALITY CORRELATION REPORT: pH & ELECTRICAL CONDUCTIVITY")
    lines.append("Metrics: Pearson Correlation (Linear) & Spearman Rank Correlation (Monotonic)")
    lines.append("Dataset: merged_locations_monthly_stacked.csv")
    lines.append("Period: 1992-01-01 to 2026-09-01 (Monthly Data)")
    lines.append("=" * 95)
    lines.append("")

    stations = [
        ("130355A", "Dee River at Kenbula"),
        ("130335A", "Dee River at Wura"),
    ]

    for station_code, station_name in stations:
        sub_df = df[df["Station_Code"] == station_code].copy()
        lines.append("\n" + "#" * 95)
        lines.append(f"LOCATION: {station_name} (Station Code: {station_code})")
        lines.append(f"Total Observations: {len(sub_df)} monthly records")
        lines.append("#" * 95)

        # 1. Direct Relationship: pH vs. Electrical Conductivity
        lines.append("\n" + "-" * 90)
        lines.append("1. DIRECT RELATIONSHIP: pH vs. ELECTRICAL CONDUCTIVITY")
        lines.append("-" * 90)
        lines.append(
            f"{'Target Variable 1':<30} {'Target Variable 2':<28} {'Pearson r':>11} {'Spearman rho':>14} {'N':>6}"
        )
        lines.append("-" * 90)

        ec_cols = [c for c in target_measurements if "E Conduct." in c]
        ph_cols = [c for c in target_measurements if "pH" in c]

        for ec_col in ec_cols:
            for ph_col in ph_cols:
                valid = sub_df[ec_col].notna() & sub_df[ph_col].notna()
                n = int(np.sum(valid))
                if n > 2:
                    s1 = sub_df.loc[valid, ec_col]
                    s2 = sub_df.loc[valid, ph_col]
                    r_pearson = s1.corr(s2, method="pearson")
                    r_spearman = s1.corr(s2, method="spearman")
                    p_str = f"{r_pearson:+.4f}"
                    s_str = f"{r_spearman:+.4f}"
                else:
                    p_str, s_str = "N/A", "N/A"
                lines.append(f"{ec_col:<30} {ph_col:<28} {p_str:>11} {s_str:>14} {n:>6}")

        # 2. Correlations with Other Physical Variables
        lines.append("\n" + "-" * 90)
        lines.append("2. CORRELATIONS WITH OTHER VARIABLES (Physical Measurements)")
        lines.append("-" * 90)

        for target in target_measurements:
            lines.append(f"\n[Target: {target}]")
            lines.append(
                f"{'Other Variable':<34} {'Pearson r':>11} {'Spearman rho':>14} {'Sample Size (N)':>18}"
            )
            lines.append("-" * 80)

            results = []
            for other in other_measurements:
                valid = sub_df[target].notna() & sub_df[other].notna()
                n = int(np.sum(valid))
                if n > 2:
                    s_t = sub_df.loc[valid, target]
                    s_o = sub_df.loc[valid, other]
                    r_pearson = s_t.corr(s_o, method="pearson")
                    r_spearman = s_t.corr(s_o, method="spearman")
                else:
                    r_pearson, r_spearman = np.nan, np.nan
                results.append((other, r_pearson, r_spearman, n))

            results.sort(
                key=lambda x: -1 if np.isnan(x[1]) else abs(x[1]),
                reverse=True,
            )

            for other, rp, rs, n in results:
                p_display = f"{rp:+.4f}" if not np.isnan(rp) else "       NaN"
                s_display = f"{rs:+.4f}" if not np.isnan(rs) else "       NaN"
                lines.append(f"{other:<34} {p_display:>11} {s_display:>14} {n:>18}")

        # 3. Compact Comparison Table
        lines.append("\n" + "-" * 90)
        lines.append("3. COMPACT SUMMARY: EC Mean & pH Mean vs. Key Environmental Variables")
        lines.append("-" * 90)

        summary_others = [
            "Rainfall (mm)_Total",
            "Level (Metres)_Mean",
            "Discharge (Cumecs)_Mean",
            "Discharge (ML/day)_Mean",
            "Volume ML_Total",
            "Water Temp (Deg. C)_Mean",
        ]

        header = (
            f"{'Environmental Variable':<28} | "
            f"{'EC Pearson':>11} {'EC Spearman':>12} (N) | "
            f"{'pH Pearson':>11} {'pH Spearman':>12} (N)"
        )
        lines.append(header)
        lines.append("-" * 90)

        ec_mean = "E Conduct. (us/cm)_Mean"
        ph_mean = "pH (pH units)_Mean"

        for other in summary_others:
            v_ec = sub_df[ec_mean].notna() & sub_df[other].notna()
            n_ec = int(np.sum(v_ec))
            if n_ec > 2:
                ec_p = f"{sub_df.loc[v_ec, ec_mean].corr(sub_df.loc[v_ec, other], method='pearson'):+.3f}"
                ec_s = f"{sub_df.loc[v_ec, ec_mean].corr(sub_df.loc[v_ec, other], method='spearman'):+.3f}"
            else:
                ec_p, ec_s = "   N/A", "   N/A"

            v_ph = sub_df[ph_mean].notna() & sub_df[other].notna()
            n_ph = int(np.sum(v_ph))
            if n_ph > 2:
                ph_p = f"{sub_df.loc[v_ph, ph_mean].corr(sub_df.loc[v_ph, other], method='pearson'):+.3f}"
                ph_s = f"{sub_df.loc[v_ph, ph_mean].corr(sub_df.loc[v_ph, other], method='spearman'):+.3f}"
            else:
                ph_p, ph_s = "   N/A", "   N/A"

            lines.append(
                f"{other:<28} | {ec_p:>11} {ec_s:>12} ({n_ec:>3}) | {ph_p:>11} {ph_s:>12} ({n_ph:>3})"
            )

        # 4 & 5. Full Matrices (Pearson & Spearman)
        lines.append("\n" + "-" * 90)
        lines.append("4. COMPLETE PEARSON CORRELATION MATRIX (All Columns)")
        lines.append("-" * 90)
        full_pearson = sub_df[target_all].apply(
            lambda t: sub_df[other_all].apply(lambda o: t.corr(o, method="pearson"))
        )
        lines.append(full_pearson.round(4).to_string())

        lines.append("\n" + "-" * 90)
        lines.append("5. COMPLETE SPEARMAN CORRELATION MATRIX (All Columns)")
        lines.append("-" * 90)
        full_spearman = sub_df[target_all].apply(
            lambda t: sub_df[other_all].apply(lambda o: t.corr(o, method="spearman"))
        )
        lines.append(full_spearman.round(4).to_string())
        lines.append("")

    content = "\n".join(lines)
    with open(output_txt, "w", encoding="utf-8") as f:
        f.write(content)

    print(f"Both Pearson & Spearman correlations computed and written to: {output_txt}")


if __name__ == "__main__":
    compute_correlations()
