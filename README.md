# 2. Các bước thực hiện & Xử lý xung độtBước 1: Khởi tạo dữ liệu ban đầu trên nhánh mainBash# Tạo file cấu hình ban đầu
echo '{"port": 8080, "debug": false}' > config.json
git add config.json
git commit -m "init config"
Bước 2: Tạo nhánh feature-api và phát triển các commit mớiTạo nhánh feature-api và thực hiện 2 commit chỉnh sửa config.json:Bashgit checkout -b feature-api

# Commit 1: Đổi port thành 9000
Sửa config.json thành: {"port": 9000, "debug": false}
git add config.json
git commit -m "feat: change port"

# Commit 2: Bật chế độ debug
Sửa config.json thành: {"port": 9000, "debug": true}
git add config.json
git commit -m "feat: enable debug"
Bước 3: Giả lập các commit mới trên nhánh mainChuyển về nhánh main và thực hiện 2 commit chỉnh sửa đè lên cùng các thông số tệp config.json:Bashgit checkout main

# Commit 1: Cập nhật port trên main
Sửa config.json thành: {"port": 8081, "debug": false}
git add config.json
git commit -m "update port on main"

# Commit 2: Thêm cấu hình môi trường env
Sửa config.json thành: {"port": 8081, "debug": false, "env": "production"}
git add config.json
git commit -m "add env config"
Bước 4: Thực hiện git rebase main và xử lý từng chặng conflictQuay lại nhánh feature-api và tiến hành rebase:   Bashgit checkout feature-api
git rebase main
Lần Conflict 1: Ở commit feat: change portGit báo lỗi xung đột tại tệp config.json do cả main và feature-api đều sửa cấu hình port.   Mở tệp config.json, tiến hành hợp nhất thủ công (giữ lại cấu hình mới nhất phù hợp từ cả 2 phía):JSON{
  "port": 9000,
  "debug": false,
  "env": "production"
}
Đánh dấu đã giải quyết xong xung đột và tiếp tục rebase:   Bashgit add config.json
git rebase --continue
Lần Conflict 2: Ở commit feat: enable debugGit tiếp tục tạm dừng do xung đột giá trị debug khi phát lại commit tiếp theo.Mở tệp config.json và cập nhật lại nội dung chuẩn hoàn chỉnh:JSON{
  "port": 9000,
  "debug": true,
  "env": "production"
}
Đánh dấu đã giải quyết và hoàn tất rebase:   Bashgit add config.json
git rebase --continue
3. Kết quả kiểm tra1. Trạng thái sau khi hoàn tất RebaseChạy lệnh kiểm tra trạng thái:Bashgit status
Kết quả:PlaintextOn branch feature-api
nothing to commit, working tree clean
2. Kiểm tra cây lịch sử commitChạy lệnh kiểm tra lịch sử dạng đồ thị:Bashgit log --graph --oneline
Kết quả mong đợi:Lịch sử Git nằm trên một đường thẳng tắp (không có các nhánh rẽ hay Merge Commit phụ), các commit của feature-api nối tiếp ngay sau các commit mới nhất của main:Plaintext* 2da3688 (HEAD -> feature-api) feat: enable debug
* 19d1098 feat: change port
* 484d817 (main) add env config
* 81a039d update port on main
* 5c4e12a init config
