# index.html7
https://script.google.com/macros/s/AKfycbxaRafe2flN_KRzO5hPRrhwWak5gRct5aHFVXe1NnooYr2PwUotOeen7pO6cSTje-Ld7w/exec
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ร้านขายยาสระผมสมุนไพร & ระบบจัดการคลัง</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @media print {
      .no-print { display: none !important; }
      .print-only { display: block !important; }
    }
  </style>
</head>
<body class="bg-gray-50 text-gray-800">

  <!-- แถบเมนูสลับหน้า -->
  <nav class="bg-teal-600 text-white p-4 shadow no-print">
    <div class="max-w-4xl mx-auto flex justify-between items-center">
      <h1 class="font-bold text-lg">🌿 Herbal Shampoo Store</h1>
      <div class="space-x-2">
        <button onclick="switchTab('shop')" class="bg-white text-teal-700 px-3 py-1 rounded font-medium">หน้าร้าน (สั่งซื้อ)</button>
        <button onclick="switchTab('warehouse')" class="bg-teal-800 text-white px-3 py-1 rounded font-medium">ระบบคลัง (Pick & Pack / จ่าหน้า)</button>
      </div>
    </div>
  </nav>

  <!-- ส่วนที่ 1: หน้า Landing Page ขายยาสระผม -->
  <div id="shop-section" class="max-w-4xl mx-auto p-6">
    <div class="text-center mb-8">
      <h2 class="text-3xl font-bold text-teal-800">ยาสระผมสมุนไพรธรรมชาติ สูตรเข้มข้น</h2>
      <p class="text-gray-600 mt-2">ลดผมร่วง ขจัดรังแค บำรุงรากผมให้แข็งแรง</p>
    </div>

    <!-- สินค้า 2 SKUs -->
    <div class="grid md:grid-cols-2 gap-6 mb-8">
      <!-- SKU 1 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="images/shampoo1.jpg" alt="SKU 01" class="w-full h-48 object-cover rounded-lg mb-4 bg-gray-200">
        <span class="text-xs bg-teal-100 text-teal-800 px-2 py-1 rounded">SKU-001</span>
        <h3 class="text-xl font-bold mt-2">สูตรมะกรูด ขจัดรังแค (300 ml)</h3>
        <p class="text-teal-600 font-bold text-lg mt-1">ราคา 190 บาท</p>
        <div class="mt-4 flex items-center justify-between">
          <label class="font-medium">จำนวน (ขวด):</label>
          <input type="number" id="qty-sku1" value="0" min="0" onchange="calcTotal()" class="border rounded w-20 p-1 text-center">
        </div>
      </div>

      <!-- SKU 2 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="images/shampoo2.jpg" alt="SKU 02" class="w-full h-48 object-cover rounded-lg mb-4 bg-gray-200">
        <span class="text-xs bg-purple-100 text-purple-800 px-2 py-1 rounded">SKU-002</span>
        <h3 class="text-xl font-bold mt-2">สูตรอัญชัน บำรุงผมดกดำ (300 ml)</h3>
        <p class="text-purple-600 font-bold text-lg mt-1">ราคา 220 บาท</p>
        <div class="mt-4 flex items-center justify-between">
          <label class="font-medium">จำนวน (ขวด):</label>
          <input type="number" id="qty-sku2" value="0" min="0" onchange="calcTotal()" class="border rounded w-20 p-1 text-center">
        </div>
      </div>
    </div>

    <!-- ฟอร์มสั่งซื้อ -->
    <div class="bg-white p-6 rounded-xl shadow border">
      <h3 class="text-xl font-bold mb-4 border-b pb-2">ข้อมูลจัดส่งสินค้า</h3>
      <form id="order-form" onsubmit="submitOrder(event)" class="space-y-4">
        <div>
          <label class="block text-sm font-medium">ชื่อ-นามสกุล ผู้รับ</label>
          <input type="text" id="cust-name" required class="w-full border rounded p-2 mt-1" placeholder="เช่น สมชาย ใจดี">
        </div>
        <div>
          <label class="block text-sm font-medium">เบอร์โทรศัพท์</label>
          <input type="tel" id="cust-phone" required class="w-full border rounded p-2 mt-1" placeholder="08X-XXX-XXXX">
        </div>
        <div>
          <label class="block text-sm font-medium">ที่อยู่จัดส่ง (พร้อมรหัสไปรษณีย์)</label>
          <textarea id="cust-address" required rows="3" class="w-full border rounded p-2 mt-1" placeholder="บ้านเลขที่ ถนน ตำบล อำเภอ จังหวัด รหัสไปรษณีย์"></textarea>
        </div>
        <div class="bg-gray-100 p-4 rounded-lg flex justify-between items-center">
          <span class="font-bold text-lg">ยอดรวมทั้งหมด:</span>
          <span id="total-price" class="text-2xl font-bold text-teal-700">0 บาท</span>
        </div>
        <button type="submit" id="submit-btn" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-3 rounded-lg text-lg">
          ยืนยันการสั่งซื้อ
        </button>
      </form>
    </div>
  </div>

  <!-- ส่วนที่ 2: หน้าคลังสินค้า (หยิบสินค้า + แพ็ค + พิมพ์ใบจ่าหน้าพัสดุ) -->
  <div id="warehouse-section" class="max-w-4xl mx-auto p-6 hidden">
    <div class="flex justify-between items-center mb-6 no-print">
      <div>
        <h2 class="text-2xl font-bold text-gray-800">📦 ระบบหยิบสินค้า (Pick & Pack) และใบจ่าหน้าพัสดุ</h2>
        <p class="text-sm text-gray-500">ดึงข้อมูลแบบเรียลไทม์จาก Google Sheets</p>
      </div>
      <button onclick="loadOrders()" class="bg-blue-600 text-white px-4 py-2 rounded shadow hover:bg-blue-700">
        🔄 รีเฟรชออเดอร์
      </button>
    </div>

    <div id="orders-container" class="space-y-6">
      <p class="text-center text-gray-500">กำลังโหลดข้อมูล...</p>
    </div>
  </div>

  <script>
    // *** นำ Web App URL จาก Google Apps Script มาวางตรงนี้ ***
    const SCRIPT_URL = 'วาง_URL_ของ_GOOGLE_APPS_SCRIPT_ที่นี่';

    function switchTab(tab) {
      document.getElementById('shop-section').classList.toggle('hidden', tab !== 'shop');
      document.getElementById('warehouse-section').classList.toggle('hidden', tab !== 'warehouse');
      if (tab === 'warehouse') loadOrders();
    }https://script.google.com/macros/s/AKfycbxaRafe2flN_KRzO5hPRrhwWak5gRct5aHFVXe1NnooYr2PwUotOeen7pO6cSTje-Ld7w/exec

    function calcTotal() {
      const q1 = parseInt(document.getElementById('qty-sku1').value) || 0;
      const q2 = parseInt(document.getElementById('qty-sku2').value) || 0;
      const total = (q1 * 190) + (q2 * 220);
      document.getElementById('total-price').innerText = total.toLocaleString() + ' บาท';
      return total;
    }

    async function submitOrder(e) {
      e.preventDefault();
      const q1 = parseInt(document.getElementById('qty-sku1').value) || 0;
      const q2 = parseInt(document.getElementById('qty-sku2').value) || 0;
      if (q1 === 0 && q2 === 0) {
        alert('กรุณาเลือกสินค้าอย่างน้อย 1 ขวดครับ');
        return;
      }

      const btn = document.getElementById('submit-btn');
      btn.disabled = true;
      btn.innerText = 'กำลังบันทึกข้อมูล...';

      const payload = {
        name: document.getElementById('cust-name').value,
        phone: document.getElementById('cust-phone').value,
        address: document.getElementById('cust-address').value,
        sku1: q1,
        sku2: q2,
        total: calcTotal()
      };

      try {
        await fetch(SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });
        alert('สั่งซื้อสำเร็จ! ข้อมูลถูกบันทึกลง Google Sheets เรียบร้อยแล้ว');
        document.getElementById('order-form').reset();
        calcTotal();
      } catch (err) {
        alert('เกิดข้อผิดพลาด: ' + err);
      } finally {
        btn.disabled = false;
        btn.innerText = 'ยืนยันการสั่งซื้อ';
      }
    }

    async function loadOrders() {
      const container = document.getElementById('orders-container');
      container.innerHTML = '<p class="text-center text-gray-500">กำลังดึงข้อมูลจาก Google Sheets...</p>';

      try {
        const res = await fetch(SCRIPT_URL);
        const orders = await res.json();
        
        if (orders.length === 0) {
          container.innerHTML = '<p class="text-center text-gray-500">ยังไม่มีรายการสั่งซื้อ</p>';
          return;
        }

        container.innerHTML = orders.reverse().map(order => `
          <div class="bg-white border-2 border-gray-300 rounded-xl p-6 shadow-sm">
            <!-- ส่วนใบหยิบสินค้า (Picking List) -->
            <div class="bg-yellow-50 border border-yellow-200 p-4 rounded-lg mb-4">
              <div class="flex justify-between items-center border-b border-yellow-200 pb-2 mb-2">
                <span class="font-bold text-yellow-900">📋 รายการหยิบสินค้า (Picking List) - #${order.orderId}</span>
                <span class="text-xs text-gray-500">${order.timestamp}</span>
              </div>
              <ul class="space-y-1 text-sm">
                ${order.sku1 > 0 ? `<li>✅ <strong>[SKU-001]</strong> สูตรมะกรูด ขจัดรังแค : <strong>จำนวน ${order.sku1} ขวด</strong></li>` : ''}
                ${order.sku2 > 0 ? `<li>✅ <strong>[SKU-002]</strong> สูตรอัญชัน บำรุงผมดกดำ : <strong>จำนวน ${order.sku2} ขวด</strong></li>` : ''}
              </ul>
              <div class="mt-2 pt-2 border-t border-yellow-200 flex gap-4 text-xs font-medium text-gray-700">
                <label><input type="checkbox"> หยิบสินค้าครบแล้ว</label>
                <label><input type="checkbox"> แพ็คลงกล่องเรียบร้อย</label>
              </div>
            </div>

            <!-- ส่วนใบจ่าหน้าพัสดุ (Shipping Label) -->
            <div class="border-2 border-dashed border-gray-800 p-4 rounded-lg bg-white">
              <div class="flex justify-between items-start border-b pb-2 mb-2">
                <div>
                  <p class="text-xs text-gray-500">ผู้ส่ง (SENDER):</p>
                  <p class="font-bold text-sm">ร้าน Herbal Shampoo Store</p>
                  <p class="text-xs">คลังสินค้าหลัก กทม. 10900</p>
                </div>
                <div class="text-right">
                  <span class="bg-black text-white text-xs px-2 py-1 font-mono">${order.orderId}</span>
                  <p class="text-xs font-bold mt-1">ยอดชำระ: ${order.total} บาท</p>
                </div>
              </div>
              <div class="mt-3">
                <p class="text-xs text-gray-500">ผู้รับ (RECEIVER):</p>
                <p class="font-bold text-lg">${order.name} (โทร. ${order.phone})</p>
                <p class="text-base mt-1">${order.address}</p>
              </div>
            </div>

            <div class="mt-4 text-right no-print">
              <button onclick="window.print()" class="bg-gray-800 text-white px-4 py-2 rounded text-sm hover:bg-black">
                🖨️ พิมพ์ใบจ่าหน้าพัสดุ
              </button>
            </div>
          </div>
        `).join('');
      } catch (err) {
        container.innerHTML = '<p class="text-center text-red-500">โหลดข้อมูลไม่สำเร็จ ตรวจสอบการตั้งค่า Apps Script</p>';
      }
    }
  </script>
</body>
</html>
