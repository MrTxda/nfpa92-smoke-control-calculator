# Báo cáo rà soát — NFPA92_SmokControl_Calculator.html theo NFPA 92:2024

Ngày rà soát: 2026-10-12 · Người rà soát: XDA (AI assistant của MrT)
Phạm vi: toàn bộ công thức, trích dẫn điều khoản, logic kiểm tra và mã JS.

## 1. Kết luận chung

- **Các công thức cốt lõi ĐÚNG** và khớp NFPA 92 (không đổi qua các bản
  2012 → 2024): z_l, ṁ strong/flame plume, ΔT_s, ρ_s, Q_v, công suất quạt.
- Bản **2024** chỉ sửa định nghĩa damper (Ch.3), thuật ngữ thiết bị điều
  khiển (Ch.6), kiểm tra định kỳ (Ch.8) và bổ sung Annex A — **không thay
  đổi thủ tục tính toán §5.5.1**. Ghi nhãn "NFPA 92:2024" là hợp lệ.
- Phát hiện **5 trích dẫn điều khoản sai** và **1 lỗi logic** (cấp khí bù
  = 100% hút, trái §4.4.4.1.2) — đã sửa trong v1.1.
- Kiểm thử số bằng Node.js: 3 kịch bản (mặc định / atrium cao / cháy nhỏ)
  cho kết quả hợp lý; hai công thức ṁ nối tiếp nhau tại z = z_l với sai
  số 0.9% (tính liên tục tốt). Cú pháp JS: OK.

## 2. Chi tiết từng mục

| # | Nội dung | Kết quả | Ghi chú |
|---|---|---|---|
| 1 | z_l = 0.166·Q_c^0.4 | ✅ ĐÚNG | Khớp NFPA 92 §5.5.1 (Heskestad) |
| 2 | ṁ strong plume | ✅ ĐÚNG | 0.071·Q_c^(1/3)·z^(5/3) + 0.0018·Q_c |
| 3 | ṁ flame (z < z_l) | ✅ ĐÚNG | 0.032·Q_c^(3/5)·z |
| 4 | ΔT_s = Q_c/(ṁ·c_p); T_s; ρ_s = 353/(T_s+273) | ✅ ĐÚNG | Đơn vị nhất quán (kW, kg/s, kJ/kg·K) |
| 5 | Q_v = ṁ/ρ_s; Q_ex = Q_v·3600 | ✅ ĐÚNG | |
| 6 | P = Q_v·ΔP/η | ✅ ĐÚNG | |
| 7 | X_c mặc định 0.70 | ✅ ĐÚNG | NFPA 92 §5.5.1.4 quy định dùng 0.7 khi chưa có số liệu thử nghiệm |
| 8 | Trích dẫn "§5.5.6 / §5.5.6.2" cho cấp khí bù | ❌ SAI → **ĐÃ SỬA** | Đúng là **§4.4.4.1**: miệng cấp dưới mặt phân cách lớp khói (§4.4.4.1.1); **cấp bù cơ khí < lưu lượng hút** (§4.4.4.1.2); v ≤ 1.02 m/s (§4.4.4.1.4) |
| 9 | Q_makeup = 100% Q_exhaust | ❌ SAI logic → **ĐÃ SỬA** | Đổi thành **90%** Q_exhaust (thực hành 85–95%, phần còn lại là rò lọt), đúng §4.4.4.1.2 |
| 10 | Trích dẫn "§4.4.3" cho giới hạn 200°C | ❌ SAI → **ĐÃ SỬA** | §4.4.3 là "Fire Location". 200°C là **tiêu chí duy trì được (tenability)** theo NFPA 92 Annex — đã sửa nhãn, giữ logic kiểm tra |
| 11 | Trích dẫn "§5.5.5" cho quạt 250°C/2h | ❌ SAI → **ĐÃ SỬA** | §5.5.5 là lưu lượng thể tích/Ks. Đổi thành NFPA 92 Ch.6 (thiết bị) + ghi rõ 250°C/2h là thực hành theo EN 12101-3 |
| 12 | Trích dẫn "NFPA 72 Annex B" cho HRR | ❌ SAI → **ĐÃ SỬA** | Đúng là **NFPA 92 Annex B** (dự báo tốc độ tỏa nhiệt) |
| 13 | A_f "dùng để xác định kích thước đám cháy" | ⚠️ GÂY HIỂU LẦM → **ĐÃ SỬA** | A_f không tham gia công thức. Đã sửa tooltip (thông số hồ sơ) và thêm thẻ KPI "Mật độ cháy thiết kế q = Q/A_f" |
| 14 | Kiểm tra z/H < 80% trích dẫn "§4.4" | ⚠️ MƠ HỒ → **ĐÃ SỬA** | Diễn đạt lại thành độ sâu lớp khói (H−z) ≥ 20%H, trích dẫn đúng **§4.5.1.3** |
| 15 | z_min = 1.8 m trích dẫn "§4.4.1" | ⚠️ CHƯA XÁC MINH → **ĐÃ SỬA** | Đổi thành "tiêu chí thiết kế NFPA 92 / IBC §909.8.1 (≥ 6 ft ≈ 1.83 m trên mặt đi bộ)" |
| 16 | Số quạt tối thiểu 2 | ✅ OK | Khuyến nghị thiết kế (dự phòng), không ghi là yêu cầu bắt buộc của tiêu chuẩn |
| 17 | Biến `pct` không dùng | 🧹 DỌN | Đã xóa |

## 3. Bổ sung v1.2 (2026-10-12) — cấp bù theo khối lượng

Phát hiện: v1.1 tính Q_makeup = 90% × Q_exhaust **theo thể tích**, trong khi
NFPA 92 §4.4.4.1.2 yêu cầu so sánh theo **khối lượng**. Vì khí thải ở T_s
và khí bù ở T_a (≠ T_s) nên ρ_s ≠ ρ_a — lấy % thể tích cho kết quả sai.

Ví dụ với số mặc định (Q = 5 000 kW, z = 3.2 m, T_a = 30°C):
ṁ_p = 13.70 kg/s, T_s = 284°C → ρ_s = 0.634, ρ_a = 1.165 kg/m³.

| Cách tính | Q_makeup | Khối lượng cấp bù | So với hút |
|---|---|---|---|
| v1.1 (thể tích 90%) | 70 067 m³/h | 22.67 kg/s | **166% — VI PHẠM §4.4.4.1.2** |
| v1.2 (khối lượng 90%) | 38 103 m³/h | 12.33 kg/s | 90% — ĐẠT |

Đã sửa: ṁ_mk = 0.90 × ṁ_p; Q_mk = ṁ_mk / ρ_a × 3600 (ρ_a tại T_a);
thêm ρ_a vào bảng tính/KPI/CSV; số quạt, tiết diện ống cấp bù và diện
tích thông thủy miệng cấp (v ≤ 1.02 m/s, §4.4.4.1.4) tính riêng theo
thể tích tại T_a.


## 4. Bổ sung v1.3 (2026-10-12) — tab TCVN·QCVN

Đối chiếu văn bản gốc TCVN 5687:2024 (bản PDF, tra cứu 2026-10-12):

- **Phân loại áp dụng**: QCVN 06:2022/BXD + SĐ1:2023, Phụ lục D.2 (a–h)
  — hành lang/sảnh/phòng/gara; kèm các miễn trừ SĐ1:2023 (nhà F4 ≤ 6 tầng
  có báo cháy đầu báo khói/chữa cháy tự động; hạng D, E không yêu cầu).
  Tăng áp & cấp bù theo D.10, D.12 (vùng 20–50 Pa).
- **Phụ lục H** (tham khảo): H.1 hành lang/sảnh
  (G1 = 3420·B·n·H^1.5 nhà ở / 4300·B·n·H^1.5·Kd nhà công cộng, n nội suy
  Bảng H.1); H.2 gian phòng (G = 678,8·Pf·Y^1,5·Ks); H.3 thoát khói tự nhiên
  qua cửa (G1 = 3564·Ad·√[h0(γv−γ)ρv² + 0,7v²ρv²]·Ks). Đổi kg/h → m³/h theo
  nhiệt độ khói do người dùng nhập (mặc định 300°C).
- **Rò lọt**: Đ.7.4b Gv = 40,3·√(Av·ΔP)·n (van đóng); Bảng 1
  q = C·pt^0,65·10⁻³ m³/s·m² (cấp B cho ống gió).
- **Cấp bù**: Phụ lục G.7.4 — Ga = Gsm/(1−n) (G.10), La = Gsm/ρa (G.11),
  La = Lsm·Ta/Tsm (G.12); G.7.1 khuyến nghị 85–95%.
- **Tăng áp**: v ≥ 1,3 m/s qua cửa mở (Đ.6.8.6b) → Q = 1,3·A_cửa;
  Bảng G.1 chênh lệch áp suất vùng khói (12,5/25/35/45 Pa).
- **Khí động ống gió**: SMACNA — Darcy–Weisbach + Haaland (ε = 0,09 mm
  tôn mạ kẽm), đường kính tương đương Huebscher cho ống chữ nhật,
  tổn thất cục bộ Σζ·ρv²/2 (bảng 12 phụ kiện); TCVN 232:1999 chỉ quy định
  chế tạo/lắp đặt/nghiệm thu (không có phần tính khí động).
- Kiểm thử số 2026-10-12: nội suy Bảng H.1 khớp điểm bảng; ống 15 000 m³/h
  → 850×600 mm @ 8,17 m/s, 0,76 Pa/m (đẳng ma sát 1,0); đoạn ống 25 m +
  phụ kiện → ΔP = 119 Pa — hợp lý.

## 5. Bổ sung v1.4 (2026-10-12) — tab TCVN·QCVN lên trước, tăng áp nâng cấp

- Tab **TCVN·QCVN hiển thị trước (mặc định)**, tab NFPA 92 chuyển sau.
- Tăng áp thang/khoang đệm tính đầy đủ: Q_mở = N_mở·A_cửa·v (v ≥ 1,3 m/s,
  Đ.6.8.6b) + rò lọt qua khe N cửa đóng q = Cd·A_khe·√(2ΔP/ρ) (Đ.6.8.6b
  yêu cầu kể đến rò lọt khe cửa) + dự phòng rò lọt hệ thống; tự kiểm tra
  ΔP ∈ [20, 50] Pa (D.12) và lực mở cửa F ≈ ΔP·A/2 ≤ 100 N (D.1.6).
- Kiểm thử: 20 tầng × 2 cửa, mở 2 cửa, ΔP = 30 Pa → Q_quạt = 34 805 m³/h
  (17 690 qua cửa mở + 12 575 rò lọt 38 cửa đóng), F = 28,3 N — hợp lý.

## 6. Việc chưa làm (ngoài phạm vi bảng tính hiện tại)


- Kiểm tra plugholing / số lượng & khoảng cách miệng hút (§5.6)
- Cột khói tràn ban công (balcony spill plume, §5.5.2) và cột khói gần tường
- Phân tích sơ tán ASET/RSET, tầm nhìn trong khói
