# NFPA 92:2024 — Bảng tính hệ thống hút khói sự cố

Bảng tính thiết kế sơ bộ hệ thống hút khói cơ khí cho không gian lớn
(atrium, sảnh, TTTM, kho…) theo mô hình **cột khói đối xứng trục
(axisymmetric plume)** của **NFPA 92:2024 §5.5.1**.

Mở file `NFPA92_SmokControl_Calculator.html` bằng trình duyệt — không cần
cài đặt, không cần mạng. Nhấn **TÍNH TOÁN** để xem kết quả, **Xuất CSV**
để lưu hồ sơ.

## Phương pháp tính

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
| Cấp khí bù | Q_makeup = 90% · Q_ex | §4.4.4.1.2 (cấp bù cơ khí < hút) |
| Công suất quạt | P = Q_v · ΔP / η | — |

## Các tiêu chí kiểm tra tự động

- **Chiều cao lớp khói sạch** z ≥ 2.1 m khuyến nghị (1.8 m tuyệt đối trên
  mặt đi bộ của lối thoát nạn) — tiêu chí thiết kế NFPA 92 / IBC §909.8.1
- **Nhiệt độ khói** T_s ≤ 200°C — tiêu chí duy trì được (tenability),
  NFPA 92 Annex
- **Độ sâu lớp khói** (H − z) ≥ 20% chiều cao H — §4.5.1.3
- **Cấp khí bù**: miệng cấp dưới mặt phân cách lớp khói (§4.4.4.1.1),
  vận tốc ≤ 1.02 m/s tại vùng có thể chạm cột khói (§4.4.4.1.4)
- **Thiết bị** chịu được nhiệt độ khói tính toán (NFPA 92 Ch.6; cấp
  250°C/2h là thực hành phổ biến theo EN 12101-3)

## Lịch sử rà soát

- **v1.1 (2026-10-12)** — Rà soát toàn bộ công thức và trích dẫn theo
  NFPA 92:2024. Chi tiết xem `KIEM-TRA-NFPA92-2024.md`.

## Lưu ý

Kết quả mang tính tham khảo kỹ thuật cho giai đoạn thiết kế sơ bộ.
Cần xác nhận bởi kỹ sư có kinh nghiệm và kiểm tra thêm: tầm nhìn trong
khói, thời gian sơ tán (ASET/RSET), bố trí miệng hút theo mặt bằng thực
tế, plugholing/số lượng miệng hút (§5.6).
