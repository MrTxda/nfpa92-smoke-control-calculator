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

## 3. Việc chưa làm (ngoài phạm vi bảng tính hiện tại)

- Kiểm tra plugholing / số lượng & khoảng cách miệng hút (§5.6)
- Cột khói tràn ban công (balcony spill plume, §5.5.2) và cột khói gần tường
- Phân tích sơ tán ASET/RSET, tầm nhìn trong khói
