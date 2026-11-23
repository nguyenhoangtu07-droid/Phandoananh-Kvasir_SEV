# So sánh các mô hình học sâu phân đoạn polyp đại tràng khi dữ liệu huấn luyện hạn chế

*Comparing deep learning models for colorectal polyp segmentation under limited training data*

Kho này chứa mã nguồn, cách chia dữ liệu và toàn bộ kết quả thực nghiệm của bài báo cùng tên (đang gửi phản biện,
Tạp chí Khoa học Đại học Mở Thành phố Hồ Chí Minh – Kỹ thuật và Công nghệ).

Bài báo so sánh U-Net, U-Net++, DeepLabV3+ (bộ mã hóa ResNet-34) và SegFormer (MiT-B2) khi huấn luyện với 25%, 50% và
100% dữ liệu Kvasir-SEG, mỗi cấu hình lặp lại 3 lần, kiểm tra trên Kvasir-SEG và CVC-ClinicDB. Thí nghiệm bổ sung
dùng U-Net (ResNet-50) và SegFormer-B0 để tách ảnh hưởng của kiến trúc khỏi kích thước bộ mã hóa.

## Cấu trúc

```
notebooks/
  01_huan_luyen_4_mo_hinh.ipynb          # thực nghiệm chính: 4 mô hình × 3 mức dữ liệu × 3 seed = 36 lần huấn luyện
  02_thi_nghiem_bo_sung_bo_ma_hoa.ipynb  # thí nghiệm bổ sung: 2 mô hình × 2 mức dữ liệu × 3 seed = 12 lần
ket_qua/
  chia_tap.json                          # tên ảnh trong tập huấn luyện/kiểm định/kiểm tra và các tập con 25%, 50%
  lan_chay_1/                            # kết quả báo cáo trong bài (Bảng 2–5, Hình 2–3)
    tat_ca_lan_chay.csv                  # 36 dòng: Dice, IoU trên Kvasir-SEG và CVC-ClinicDB của từng lần chạy
    runs/                                # mỗi lần chạy: *_kvasir.csv, *_cvc.csv (Dice/IoU từng ảnh), *_lich_su.csv, *.json
    Bang2_tham_so.xlsx, Bang3_Kvasir.xlsx, Bang4_Wilcoxon.xlsx, Bang5_CVC.xlsx
  bo_sung/                               # thí nghiệm bổ sung (Bảng 6, Hình 4)
    runs/                                # 12 lần chạy của U-Net (ResNet-50) và SegFormer-B0
    ket_hop_lan_chay_1_va_bo_sung.csv    # 48 dòng: lần chạy 1 + thí nghiệm bổ sung
    Bang6_bo_ma_hoa.csv
  lan_chay_2/
    tat_ca_lan_chay_chay_lai.csv         # chạy lại toàn bộ 36 lần huấn luyện (xem mục Tính lặp lại)
hinh/                                    # Hình 1–4 của bài báo (300 dpi)
```

Tên tệp trong `runs/` có dạng `<mô hình>_<phần trăm dữ liệu>_<seed>`, trong đó `p` thay cho dấu `+`
(ví dụ `UNetpp` là U-Net++, `DeepLabV3p` là DeepLabV3+).

## Dữ liệu

Kho này **không** chứa ảnh. Hai bộ dữ liệu công khai cần tải từ nguồn gốc:

- Kvasir-SEG (Jha et al., 2020): https://datasets.simula.no/kvasir-seg/
- CVC-ClinicDB (Bernal et al., 2015): https://polyp.grand-challenge.org/CVCClinicDB/

Trên Kaggle có thể dùng các bộ `debeshjha1/kvasirseg` và `balraj98/cvcclinicdb`.

## Chạy lại thực nghiệm

1. Trên Kaggle: **Code → New Notebook → File → Import Notebook**, chọn notebook trong thư mục `notebooks/`.
2. **Add Input** hai bộ dữ liệu trên; bật **GPU** và **Internet**.
3. Chạy thử với `CHAY_THU = True`, sau đó đặt `CHAY_THU = False` và chạy toàn bộ
   (notebook 01 mất khoảng 8–14 giờ trên GPU T4; notebook 02 khoảng 2–3 giờ và cần thêm kết quả của notebook 01 làm Input).

Môi trường đã dùng: Kaggle, GPU NVIDIA Tesla T4, Python 3.13, PyTorch 2.11. Danh sách thư viện ở `requirements.txt`.

## Tính lặp lại

Huấn luyện trên GPU không hoàn toàn tất định. Khi chạy lại toàn bộ 36 lần huấn luyện (`ket_qua/lan_chay_2/`), Dice của
từng lần chạy chênh lệch trung bình 0,012 (tối đa 0,043) so với lần chạy 1. SegFormer vẫn đạt Dice cao nhất ở mọi mức
dữ liệu và trên CVC-ClinicDB; thứ tự về mức sụt giảm giữa DeepLabV3+ và SegFormer thì thay đổi. Bài báo báo cáo
lần chạy 1.

## Trích dẫn

Thông tin trích dẫn sẽ được cập nhật khi bài báo được xuất bản.

## Giấy phép

Mã nguồn theo giấy phép MIT (xem `LICENSE`). Các tệp kết quả và hình theo giấy phép CC BY 4.0.
Ảnh trong Hình 3 thuộc bộ dữ liệu Kvasir-SEG và tuân theo điều khoản sử dụng của bộ dữ liệu đó.
