# DIFRI — Rumus dan definisi metodologis

Dokumen ini merangkum rumus yang dipakai pipeline utama paper `DIFRI_7_tahap ... v6.1.1` yang diunggah. Rumus juga disertakan pada menu **Methodology & limits** di `index.html`.

## Notasi
- `a`: aplikasi; `s`: sektor; `m`: bulan; `q`: pilar risiko.
- `x`: jumlah review berlabel positif; `n`: jumlah review dalam unit app-month.
- `SF`: Security/Fraud; `DP`: Data/Privacy; `TO`: Transaction/Operational; `CP`: Consumer Protection.
- `α, β`: parameter prior Beta untuk sektor × bulan × pilar.

## Rumus inti
1. **Konsensus anotasi:** rata-rata label > 0.5 → 1; < 0.5 → 0; tepat 0.5 atau seluruh label kosong → NA.
2. **Cohen's kappa:** `κ=(p_o-p_e)/(1-p_e)`.
3. **TF-IDF:** `TF × log10(N_train/df_t)`; kosakata dan IDF dipasang pada train saja.
4. **Precision, recall, F1:** `P=TP/(TP+FP)`, `R=TP/(TP+FN)`, `F1=2PR/(P+R)`.
5. **PR-AUC / average precision:** urutkan skor menurun; jumlahkan precision pada posisi label positif, dibagi jumlah positif.
6. **ROC-AUC rank form:** `(ΣR_i+ − n+(n++1)/2)/(n+n−)` dengan average rank untuk ties.
7. **Prevalensi mentah:** `p_hat=x/n` pada app-month × pilar.
8. **Beta-Binomial:** `X|p ~ Binomial(n,p)`, `p ~ Beta(α,β)`, posterior mean `p_tilde=(x+α)/(n+α+β)`.
9. **Prior marginal likelihood:** `ℓ(α,β)=Σ[log B(x_i+α,n_i−x_i+β)−log B(α,β)]`; parameter dioptimalkan pada log scale dengan beberapa start.
10. **Fallback weighted-moment:** `p_bar=Σx/Σn`, `w_i=n_i/Σn`, `V=Σw_i(p_hat_i−p_bar)^2`, `V_bin=Σw_i p_bar(1−p_bar)/n_i`; `ρ=clip(max(V−V_bin,0)/[p_bar(1−p_bar)Σw_i(n_i−1)/n_i], 10^-4, 0.95)`; `c=min(1/ρ−1, cap)`; `α=p_bar c`, `β=(1−p_bar)c`. Cap utama 10.000; sensitivitas 1.000. Fallback aktif jika optimisasi gagal, non-konvergen, menyentuh batas, atau konsentrasi ekstrem.
11. **Posterior EB cap alternatif:** `(x+α_cap)/(n+α_cap+β_cap)`.
12. **Sektor:** `P_smq=(1/A_sm)Σ_a p_tilde_asmq`; skor pilar `100×P_smq`.
13. **Nasional:** `P_mq=(1/K_m)Σ_s P_smq`; `DIFRI_m=(SF_m+DP_m+TO_m+CP_m)/4`.
14. **Perubahan bulanan:** `Δ_m=DIFRI_m−DIFRI_(m−1)`; perubahan proporsional `DIFRI_m/DIFRI_(m−1)−1`.
15. **Geometric sensitivity:** `(SF×DP×TO×CP)^(1/4)`; jika komponen ≤0 hasil alternatif 0, jika non-finite NA.
16. **Review-weighted sensitivity:** `P_RW_mq=Σx/Σn`; rata-rata empat skor pilar. Ini estimand alternatif.
17. **Probability-weighted sensitivity:** `e=Σπ_hat_j`; `p_tilde_prob=(e+α)/(n+α+β)`, lalu agregasi app→sektor→nasional dan rata-rata empat pilar. Bukan indeks utama hard-label.
18. **Monte Carlo:** `v=Σπ_hat_j(1−π_hat_j)`; `x_draw=round(N(e,√v))`, dibatasi ke `[0,n]`; 2.000 draw, prior EB tetap, hitung ulang indeks. P2.5–P97.5 hanya untuk alternatif probability-weighted, bukan CI DIFRI utama.
19. **Sensitivitas:** `MAE=mean(|DIFRI_alt−DIFRI_main|)` dan korelasi Pearson dengan seri utama, disertai mean/median/SD.