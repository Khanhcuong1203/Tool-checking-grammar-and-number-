# Tool-checking-grammar-and-number-
I want to create this tool for auditor who work with multi-language that they can check the report easily.
idk how to do it right now
i think vibe code will help but I still have no idea about it
Now i will add some code in this, pls help me if u can
import re

def extract_numbers(text):
    """
    Hàm này dùng Regular Expression (Regex) để quét và rút trích 
    tất cả các con số từ một đoạn văn bản.
    Hỗ trợ cả số nguyên và số thập phân/hàng nghìn có dấu phẩy hoặc chấm.
    Ví dụ: "1,000", "500.5", "2026"
    """
    # Pattern tìm các chuỗi số (có thể chứa dấu phẩy hoặc dấu chấm)
    pattern = r'\d+(?:[.,]\d+)*'
    numbers = re.findall(pattern, text)
    
    # Chuẩn hóa số (xóa dấu phẩy để dễ so sánh toán học nếu cần, ở đây tạm giữ nguyên dạng chuỗi để đối chiếu)
    return numbers

def compare_report_numbers(text_version_1, text_version_2):
    """
    Hàm so sánh hai tập hợp số liệu từ 2 bản báo cáo.
    """
    nums_v1 = extract_numbers(text_version_1)
    nums_v2 = extract_numbers(text_version_2)
    
    # Tìm các số có trong bản 1 nhưng không có trong bản 2 và ngược lại
    missing_in_v2 = [num for num in nums_v1 if num not in nums_v2]
    missing_in_v1 = [num for num in nums_v2 if num not in nums_v1]
    
    print("=== KẾT QUẢ ĐỐI CHIẾU SỐ LIỆU ===")
    print(f"Tổng số lượng con số tìm thấy ở Bản 1: {len(nums_v1)}")
    print(f"Tổng số lượng con số tìm thấy ở Bản 2: {len(nums_v2)}")
    print("-" * 30)
    
    if not missing_in_v2 and not missing_in_v1 and len(nums_v1) == len(nums_v2):
        print("Trạng thái: KHỚP 100%. Không tìm thấy sự sai lệch về số liệu.")
    else:
        print("Trạng thái: PHÁT HIỆN LỆCH SỐ LIỆU!")
        if missing_in_v2:
            print(f"-> Các số có ở Bản 1 nhưng mất/sai ở Bản 2: {missing_in_v2}")
        if missing_in_v1:
            print(f"-> Các số có ở Bản 2 nhưng mất/sai ở Bản 1: {missing_in_v1}")

# ==========================================
# CHẠY THỬ NGHIỆM (TESTING)
# ==========================================

# Giả lập copy/paste từ file báo cáo Tiếng Việt
report_vn = """
Doanh thu thuần về bán hàng và cung cấp dịch vụ năm 2025 là 15,500,000 VND.
Chi phí quản lý doanh nghiệp là 2,300,500 VND. Lợi nhuận sau thuế đạt 5,000,000 VND.
"""

# Giả lập copy/paste từ file báo cáo Tiếng Anh (Cố tình gõ sai số lợi nhuận và thiếu chi phí)
report_en = """
Net revenue from sales and services in 2025 is 15,500,000 VND.
Profit after tax reached 5,000,100 VND.
"""

compare_report_numbers(report_vn, report_en)
