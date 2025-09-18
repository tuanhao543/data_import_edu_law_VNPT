# data_import_edu_law_VNPT
# csv_output_quyetdinh
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquan.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES VĂN BẢN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES ĐIỀU ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_dieu.csv' AS row
MERGE (d:Dieu {dieuId: row['dieuId:ID']})
SET d.soDieu = row.soDieu, d.noiDung = row.noiDung;

//--- TẢI NODES ĐÍNH CHÍNH ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_dinhchinh.csv' AS row
MERGE (dc:DinhChinh {dinhchinhId: row['dinhchinhId:ID']})
SET dc.viTri = row.viTri, dc.noiDungGoc = row.noiDungGoc, dc.noiDungSua = row.noiDungSua;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// Mối quan hệ BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// Mối quan hệ DINH_CHINH_CHO
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'DINH_CHINH_CHO'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:DINH_CHINH_CHO]->(end);

// Mối quan hệ CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// Mối quan hệ BAO_GOM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Dieu {dieuId: row[':END_ID']})
MERGE (start)-[:BAO_GOM]->(end);

// Mối quan hệ CHI_TIET_DINH_CHINH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHI_TIET_DINH_CHINH'
MATCH (start:Dieu {dieuId: row[':START_ID']})
MATCH (end:DinhChinh {dinhchinhId: row[':END_ID']})
MERGE (start)-[:CHI_TIET_DINH_CHINH]->(end);

# csv_output_kehoach
//--- TẢI NODES CƠ QUAN/TỔ CHỨC (Cập nhật) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquan.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.tieuDe = row.tieuDe, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.tenNhiemVu = row.tenNhiemVu, nv.thoiGianHoanThanh = row.thoiGianHoanThanh, nv.noiDung = row.noiDung;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// BAO_GOM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:BAO_GOM]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CHU_TRI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHU_TRI'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CHU_TRI]->(end);

// PHOI_HOP
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'PHOI_HOP'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:PHOI_HOP]->(end);

# create first nodes
// Tạo node cha (chủ đề) mới có tên là "Luật Giáo dục"
MERGE (c:ChuDe {ten: 'Luật Giáo dục'})
SET c.chuDeId = 'LUAT_GIAO_DUC' // Đặt một ID duy nhất để dễ dàng tham chiếu
RETURN c

// Tìm node chủ đề và 2 node văn bản gốc
MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan) 
WHERE vb.vanbanId IN ['2904/QĐ-BGDĐT', '3632/KH-SGDĐT']

// Tạo mối quan hệ từ mỗi văn bản đến chủ đề
MERGE (vb)-[r:THUOC_CHU_DE]->(c)
RETURN vb.vanbanId AS SoHieuVanBan, type(r) AS LoaiQuanHe, c.ten AS TenChuDe

# csv_output_kehoach_904
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquantochuc.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả căn cứ) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.tenNhiemVu = row.tenNhiemVu, nv.thoiGianHoanThanh = row.thoiGianHoanThanh, nv.ketQua = row.ketQua;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// CO_MUC
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_MUC'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:CO_MUC]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CHU_TRI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHU_TRI'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CHU_TRI]->(end);

// PHOI_HOP
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'PHOI_HOP'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:PHOI_HOP]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '904/KH-BGDĐT'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)

# csv_output_congvan_613_huong_dan
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquantochuc.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả căn cứ và trích dẫn) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.tieuDe = row.tieuDe, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.doiTuong = row.doiTuong, nv.noiDung = row.noiDung;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// TRICH_DAN
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'TRICH_DAN'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:TRICH_DAN]->(end);

// CO_MUC
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_MUC'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:CO_MUC]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CO_TRACH_NHIEM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_TRACH_NHIEM'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CO_TRACH_NHIEM]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '613/SGDĐT-GDTH'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)

# csv_output_kehoach_213
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquantochuc.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả căn cứ) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.tieuDe = row.tieuDe, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.tenNhiemVu = row.tenNhiemVu, nv.thoiGianHoanThanh = row.thoiGianHoanThanh, nv.ketQua = row.ketQua;

//--- TẢI NODES PHỤ LỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_phuluc.csv' AS row
MERGE (pl:PhuLuc {phulucId: row['phulucId:ID']})
SET pl.soPhuLuc = row.soPhuLuc, pl.tenPhuLuc = row.tenPhuLuc, pl.noiDung = row.noiDung;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// CO_MUC
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_MUC'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:CO_MUC]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CHU_TRI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHU_TRI'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CHU_TRI]->(end);

// PHOI_HOP
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'PHOI_HOP'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:PHOI_HOP]->(end);

// KEM_THEO
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KEM_THEO'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:PhuLuc {phulucId: row[':END_ID']})
MERGE (start)-[:KEM_THEO]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '213/KH-BGDĐT'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)


# csv_output_congvan_119
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquantochuc.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả trích dẫn) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.tieuDe = row.tieuDe, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.doiTuong = row.doiTuong, nv.noiDung = row.noiDung;

//--- TẢI NODES PHỤ LỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_phuluc.csv' AS row
MERGE (pl:PhuLuc {phulucId: row['phulucId:ID']})
SET pl.soPhuLuc = row.soPhuLuc, pl.tenPhuLuc = row.tenPhuLuc;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// TRICH_DAN
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'TRICH_DAN'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:TRICH_DAN]->(end);

// CO_MUC
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_MUC'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:CO_MUC]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CO_TRACH_NHIEM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_TRACH_NHIEM'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CO_TRACH_NHIEM]->(end);

// KEM_THEO
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KEM_THEO'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:PhuLuc {phulucId: row[':END_ID']})
MERGE (start)-[:KEM_THEO]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '119/BGDĐT-GDTH'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)


# csv_output_tailieukythuat
//--- TẢI NODE TÀI LIỆU KỸ THUẬT ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_tailieukythuat.csv' AS row
MERGE (t:TaiLieuKyThuat {tailieuId: row['tailieuId:ID']})
SET t.loaiVanBan = row.loaiVanBan, t.soHieu = row.soHieu, t.trichYeu = row.trichYeu, t.coQuanBanHanh = row.coQuanBanHanh, t.ngayBanHanh = row.ngayBanHanh, t.noiBanHanh = row.noiBanHanh, t.phienBan = row.phienBan;

//--- TẢI NODES VĂN BẢN (viện dẫn) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu, v.ngayBanHanh = row.ngayBanHanh;

//--- TẢI NODES THUẬT NGỮ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_thuatngu.csv' AS row
MERGE (tn:ThuatNgu {thuatnguId: row['thuatnguId:ID']})
SET tn.vietTat = row.vietTat, tn.moTa = row.moTa;

//--- TẢI NODES CHƯƠNG/MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_chuongmuc.csv' AS row
MERGE (cm:ChuongMuc {chuongmucId: row['chuongmucId:ID']})
SET cm.soMuc = row.soMuc, cm.tieuDe = row.tieuDe, cm.noiDung = row.noiDung;

//--- TẢI NODES PHỤ LỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_phuluc.csv' AS row
MERGE (pl:PhuLuc {phulucId: row['phulucId:ID']})
SET pl.soPhuLuc = row.soPhuLuc, pl.noiDung = row.noiDung;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// VIEN_DAN (Từ Tài liệu kỹ thuật -> Văn bản)
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'VIEN_DAN'
MATCH (start:TaiLieuKyThuat {tailieuId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:VIEN_DAN]->(end);

// DINH_NGHIA (Từ Tài liệu kỹ thuật -> Thuật ngữ)
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'DINH_NGHIA'
MATCH (start:TaiLieuKyThuat {tailieuId: row[':START_ID']})
MATCH (end:ThuatNgu {thuatnguId: row[':END_ID']})
MERGE (start)-[:DINH_NGHIA]->(end);

// BAO_GOM (Từ Tài liệu kỹ thuật -> Chương/Mục)
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM'
MATCH (start:TaiLieuKyThuat {tailieuId: row[':START_ID']})
MATCH (end:ChuongMuc {chuongmucId: row[':END_ID']})
MERGE (start)-[:BAO_GOM]->(end);

// CO_PHU_LUC (Từ Tài liệu kỹ thuật -> Phụ lục)
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_PHU_LUC'
MATCH (start:TaiLieuKyThuat {tailieuId: row[':START_ID']})
MATCH (end:PhuLuc {phulucId: row[':END_ID']})
MERGE (start)-[:CO_PHU_LUC]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (t:TaiLieuKyThuat {tailieuId: '2088/BGDĐT-GDTH-PL1'})
MERGE (t)-[:THUOC_CHU_DE]->(c)


# csv_output_kehoach_2106
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquan.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả căn cứ) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.tieuDe = row.tieuDe, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.tenNhiemVu = row.tenNhiemVu, nv.thoiGianHoanThanh = row.thoiGianHoanThanh, nv.giaiDoan = row.giaiDoan;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// BAO_GOM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:BAO_GOM]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CHU_TRI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHU_TRI'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CHU_TRI]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '2106/KH-SGDĐT'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)


# csv_output_quyetdinh_hsb
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquan.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả căn cứ) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODE QUY CHẾ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_quyche.csv' AS row
MERGE (qc:QuyChe {quycheId: row['quycheId:ID']})
SET qc.ten = row.ten;

//--- TẢI NODES CHƯƠNG ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_chuong.csv' AS row
MERGE (ch:Chuong {chuongId: row['chuongId:ID']})
SET ch.soChuong = row.soChuong, ch.tenChuong = row.tenChuong;

//--- TẢI NODES ĐIỀU (cả của Quyết định và Quy chế) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_dieu.csv' AS row
MERGE (d:Dieu {dieuId: row['dieuId:ID']})
SET d.soDieu = row.soDieu, d.tenDieu = row.tenDieu, d.noiDung = row.noiDung;

//--- TẢI NODES PHỤ LỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_phuluc.csv' AS row
MERGE (pl:PhuLuc {phulucId: row['phulucId:ID']})
SET pl.soPhuLuc = row.soPhuLuc, pl.noiDung = row.noiDung;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERge (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// BAN_HANH_KEM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH_KEM'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:QuyChe {quycheId: row[':END_ID']})
MERGE (start)-[:BAN_HANH_KEM]->(end);

// BAO_GOM_CHUONG
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM_CHUONG'
MATCH (start:QuyChe {quycheId: row[':START_ID']})
MATCH (end:Chuong {chuongId: row[':END_ID']})
MERGE (start)-[:BAO_GOM_CHUONG]->(end);

// BAO_GOM_DIEU (từ Quyết định và từ Chương)
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM_DIEU'
MATCH (start) WHERE start.vanbanId = row[':START_ID'] OR start.chuongId = row[':START_ID']
MATCH (end:Dieu {dieuId: row[':END_ID']})
MERGE (start)-[:BAO_GOM_DIEU]->(end);

// CO_PHU_LUC
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_PHU_LUC'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:PhuLuc {phulucId: row[':END_ID']})
MERGE (start)-[:CO_PHU_LUC]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '1789/QĐ-SGDĐT'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)

# csv_output_thongtu
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquan.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả căn cứ và bị thay thế) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODE QUY ĐỊNH ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_quydinh.csv' AS row
MERGE (qd:QuyDinh {quydinhId: row['quydinhId:ID']})
SET qd.ten = row.ten;

//--- TẢI NODES CHƯƠNG ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_chuong.csv' AS row
MERGE (ch:Chuong {chuongId: row['chuongId:ID']})
SET ch.soChuong = row.soChuong, ch.tenChuong = row.tenChuong;

//--- TẢI NODES ĐIỀU (cả của Thông tư và Quy định) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_dieu.csv' AS row
MERGE (d:Dieu {dieuId: row['dieuId:ID']})
SET d.soDieu = row.soDieu, d.tenDieu = row.tenDieu, d.noiDung = row.noiDung;```

//--- TẢI NODES KHOẢN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_khoan.csv' AS row
MERGE (k:Khoan {khoanId: row['khoanId:ID']})
SET k.soKhoan = row.soKhoan, k.noiDung = row.noiDung;

//--- TẢI NODES ĐIỂM ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_diem.csv' AS row
MERGE (di:Diem {diemId: row['diemId:ID']})
SET di.soDiem = row.soDiem, di.noiDung = row.noiDung;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// CAN_CU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CAN_CU'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:CAN_CU]->(end);

// THAY_THE
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'THAY_THE'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:THAY_THE]->(end);

// BAN_HANH_KEM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH_KEM'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:QuyDinh {quydinhId: row[':END_ID']})
MERGE (start)-[:BAN_HANH_KEM]->(end);

// BAO_GOM_CHUONG
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM_CHUONG'
MATCH (start:QuyDinh {quydinhId: row[':START_ID']})
MATCH (end:Chuong {chuongId: row[':END_ID']})
MERGE (start)-[:BAO_GOM_CHUONG]->(end);

// BAO_GOM_DIEU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAO_GOM_DIEU'
MATCH (start) WHERE start.chuongId = row[':START_ID'] OR start.vanbanId = row[':START_ID']
MATCH (end:Dieu {dieuId: row[':END_ID']})
MERGE (start)-[:BAO_GOM_DIEU]->(end);

// CHUA_KHOAN
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHUA_KHOAN'
MATCH (start:Dieu {dieuId: row[':START_ID']})
MATCH (end:Khoan {khoanId: row[':END_ID']})
MERGE (start)-[:CHUA_KHOAN]->(end);

// CHUA_DIEM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CHUA_DIEM'
MATCH (start:Khoan {khoanId: row[':START_ID']})
MATCH (end:Diem {diemId: row[':END_ID']})
MERGE (start)-[:CHUA_DIEM]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '27/2020/TT-BGDĐT'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)

# csv_output_613_dinh_kem_pl
//--- TẢI NODES CƠ QUAN/TỔ CHỨC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_coquantochuc.csv' AS row
MERGE (c:CoQuanToChuc {coquanId: row['coquanId:ID']})
SET c.ten = row.ten;

//--- TẢI NODES CÁ NHÂN ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_canhan.csv' AS row
MERGE (cn:CaNhan {canhanId: row['canhanId:ID']})
SET cn.hoTen = row.hoTen, cn.chucVu = row.chucVu;

//--- TẢI NODES VĂN BẢN (bao gồm cả trích dẫn) ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_vanban.csv' AS row
MERGE (v:VanBan {vanbanId: row['vanbanId:ID']})
SET v.loaiVanBan = row.loaiVanBan, v.trichYeu = row.trichYeu;

//--- TẢI NODES MỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_muc.csv' AS row
MERGE (m:Muc {mucId: row['mucId:ID']})
SET m.soMuc = row.soMuc, m.tieuDe = row.tieuDe, m.noiDung = row.noiDung;

//--- TẢI NODES NHIỆM VỤ ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_nhiemvu.csv' AS row
MERGE (nv:NhiemVu {nhiemvuId: row['nhiemvuId:ID']})
SET nv.doiTuong = row.doiTuong, nv.noiDung = row.noiDung;

//--- TẢI NODES PHỤ LỤC ---
LOAD CSV WITH HEADERS FROM 'file:///nodes_phuluc.csv' AS row
MERGE (pl:PhuLuc {phulucId: row['phulucId:ID']})
SET pl.soPhuLuc = row.soPhuLuc, pl.tenPhuLuc = row.tenPhuLuc;

//--- TẢI TẤT CẢ CÁC MỐI QUAN HỆ ---

// BAN_HANH
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'BAN_HANH'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:BAN_HANH]->(end);

// KY_BOI
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KY_BOI'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:CaNhan {canhanId: row[':END_ID']})
MERGE (start)-[:KY_BOI]->(end);

// TRICH_DAN
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'TRICH_DAN'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:VanBan {vanbanId: row[':END_ID']})
MERGE (start)-[:TRICH_DAN]->(end);

// CO_MUC
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_MUC'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:Muc {mucId: row[':END_ID']})
MERGE (start)-[:CO_MUC]->(end);

// GIAO_NHIEM_VU
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'GIAO_NHIEM_VU'
MATCH (start:Muc {mucId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:GIAO_NHIEM_VU]->(end);

// CO_TRACH_NHIEM
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'CO_TRACH_NHIEM'
MATCH (start:CoQuanToChuc {coquanId: row[':START_ID']})
MATCH (end:NhiemVu {nhiemvuId: row[':END_ID']})
MERGE (start)-[:CO_TRACH_NHIEM]->(end);

// KEM_THEO
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
WITH row WHERE row[':TYPE'] = 'KEM_THEO'
MATCH (start:VanBan {vanbanId: row[':START_ID']})
MATCH (end:PhuLuc {phulucId: row[':END_ID']})
MERGE (start)-[:KEM_THEO]->(end);

MATCH (c:ChuDe {chuDeId: 'LUAT_GIAO_DUC'})
MATCH (vb:VanBan {vanbanId: '119/BGDĐT-GDTH'})
MERGE (vb)-[:THUOC_CHU_DE]->(c)

