# NFPA 92:2024 — Bảng tính hệ thống hút khói sự cố

Bảng tính thiết kế sơ bộ hệ thống hút khói — 2 tab:

- **🇻🇳 TCVN · QCVN** (mặc định): tính toán theo tiêu chuẩn Việt Nam — xem bảng dưới.
- **🔥 NFPA 92**: mô hình cột khói đối xứng trục (axisymmetric plume), NFPA 92:2024 §5.5.1.

Mở file `NFPA92_SmokControl_Calculator.html` bằng trình duyệt — không cần
cài đặt, không cần mạng. Dùng trực tuyến tại:
https://mrtxda.github.io/nfpa92-smoke-control-calculator/

## Tab NFPA 92 — phương pháp tính

| Bước | Công thức | Nguồn |
|---|---|---|
| Nhiệt đối lưu | Q_c = X_c · Q (X_c mặc định 0.70) | §5.5.1.4 |
| Chiều cao ngọn lửa TB | z_l = 0.166 · Q_c^0.4 | §5.5.1 |
| Lưu lượng KL khói (z ≥ z_l, strong plume) | ṁ = 0.071·Q_c^(1/3)·z^(5/3) + 0.0018·Q_c | §5.5.1 |
| Lưu lượng KL khói (z < z_l, flame) | ṁ = 0.032·Q_c^(3/5)·z | §5.5.1 |
| Độ tăng nhiệt độ khói | ΔT_s = Q_c / (ṁ · c_p) | §5.5.1 |
| Nhiệt độ khói | T_s = T_a + ΔT_s | — |
| Mật độ khói | ρ_s = 353 / (T_s + 273) | khí lý tưởng, áp suất chuẩn |
| Lưu lượng hút | Q_v = ṁ / ρ_s → Q_ex = Q_v · 3600 | §5.5.1 |
| Cấp khí bù (theo khối lượng) | ṁ_mk = 90% · ṁ_p; Q_mk = ṁ_mk / ρ_a (ρ_a tại T_a) | §4.4.4.1.2 |
| Công suất quạt hút | P = Q_v · ΔP / η | — |

## Tab TCVN · QCVN — nội dung

| Module | Căn cứ |
|---|---|
| Phân loại trường hợp áp dụng hút khói | QCVN 06:2022/BXD + SĐ1:2023, PL D.2 (a–h); tăng áp/cấp bù D.10, D.12 |
| H.1 hành lang/sảnh: G1 = 3420·B·n·H^1.5 (nhà ở) / 4300·B·n·H^1.5·Kd (công cộng), n nội suy Bảng H.1 | TCVN 5687:2024 PL H.1 |
| H.2 gian phòng: G = 678.8·Pf·Y^1.5·Ks; Pf = 12 (sprinkler) hoặc 0.38√A (4–12) | PL H.2 |
| H.3 thoát khói tự nhiên: G1 = 3564·Ad·√[h0(γv−γ)ρv² + 0.7v²ρv²]·Ks | PL H.3 |
| Rò lọt van đóng: Gv = 40.3·√(Av·ΔP)·n; rò lọt ống Bảng 1: q = C·pt^0.65·10⁻³ | Đ.7.4, Bảng 1 |
| Cấp bù: Ga = Gsm/(1−n); La = Gsm/ρa; La = Lsm·Ta/Tsm | PL G.7.4 (G.10–G.12) |
| Tăng áp: Q_mở = N_mở·A·v (v ≥ 1,3) + rò lọt N_đóng cửa (q = Cd·A_khe·√(2ΔP/ρ)); kiểm tra ΔP 20–50 Pa, lực mở cửa ≤ 100 N | Đ.6.8.6, D.12, D.1.6, Bảng G.1 |
| Khí động ống: Darcy–Weisbach + Haaland (ε = 0.09 mm), De Huebscher; tổn thất cục bộ Σζ·ρv²/2 | SMACNA; TCVN 232:1999 (chế tạo/lắp đặt) |

## Các tiêu chí kiểm tra tự động (tab NFPA)

- **Chiều cao lớp khói sạch** z ≥ 2.1 m khuyến nghị (1.8 m tuyệt đối trên
  mặt đi bộ của lối thoát nạn) — tiêu chí thiết kế NFPA 92 / IBC §909.8.1
- **Nhiệt độ khói** T_s ≤ 200°C — tiêu chí duy trì được (tenability),
  NFPA 92 Annex
- **Độ sâu lớp khói** (H − z) ≥ 20% chiều cao H — §4.5.1.3
- **Cân bằng khối lượng khí bù**: ṁ_cấp = 90%·ṁ_hút < ṁ_hút — §4.4.4.1.2
- **Cấp khí bù**: miệng cấp dưới mặt phân cách lớp khói (§4.4.4.1.1),
  vận tốc ≤ 1.02 m/s tại vùng có thể chạm cột khói (§4.4.4.1.4)
- **Thiết bị** chịu được nhiệt độ khói tính toán (NFPA 92 Ch.6; cấp
  250°C/2h là thực hành phổ biến theo EN 12101-3)

## Lịch sử

- **v1.5 (2026-10-12)** — Dọn giao diện tab TCVN·QCVN: form 2 cột, thẻ song song, lưới phụ kiện gọn.
- **v1.4 (2026-10-12)** — Tab TCVN·QCVN hiển thị trước (mặc định); tăng áp tính theo số tầng, số cửa mở đồng thời, rò lọt khe cửa đóng; kiểm tra lực mở cửa.
- **v1.3 (2026-10-12)** — Thêm tab TCVN·QCVN: phân loại áp dụng QCVN 06
  PL D.2, lưu lượng khói TCVN 5687 PL H (H.1/H.2/H.3 + rò lọt), cấp bù &
  tăng áp (PL G.7, Đ.6.8.6, D.12), khí động ống gió SMACNA, checklist
  QCVN 06 PL D / TCVN 5687 Điều 7.
- **v1.2 (2026-10-12)** — Cấp khí bù chuyển sang tính theo **khối lượng**:
  ṁ_mk = 90% · ṁ_p, Q_mk = ṁ_mk / ρ_a (ρ_a tại T_a).
- **v1.1 (2026-10-12)** — Rà soát toàn bộ công thức và trích dẫn theo
  NFPA 92:2024. Chi tiết xem `KIEM-TRA-NFPA92-2024.md`.

## Lưu ý

Kết quả mang tính tham khảo kỹ thuật cho giai đoạn thiết kế sơ bộ.
Cần xác nhận bởi kỹ sư có kinh nghiệm và kiểm tra thêm: tầm nhìn trong
khói, thời gian sơ tán (ASET/RSET), bố trí miệng hút theo mặt bằng thực
tế, plugholing/số lượng miệng hút (§5.6). Checklist QCVN 06 mang tính
tham khảo — đối chiếu văn bản gốc trước khi thẩm duyệt.
