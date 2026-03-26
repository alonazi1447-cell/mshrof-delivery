// Google Apps Script v8 — توصيل حي مشرف
// التوزيع التلقائي على الموصلين
const SHEET_NAME = 'الطلبات';
const DRIVERS = ['أحمد', 'سعد', 'فهد'];

function doGet(e)  { return handle(e); }
function doPost(e) { return handle(e); }

function handle(e) {
  const p = e.parameter; let r;
  try {
    if      (p.action==='getOrders')      r = getOrders();
    else if (p.action==='addOrder')       r = addOrder(p);
    else if (p.action==='updateStatus')   r = updateStatus(p);
    else if (p.action==='driverReady')    r = driverReady(p);
    else r = {success:false, error:'unknown'};
  } catch(err) { r = {success:false, error:err.toString()}; }
  return ContentService.createTextOutput(JSON.stringify(r))
    .setMimeType(ContentService.MimeType.JSON);
}

// جلب كل الطلبات
function getOrders() {
  const s = getSheet(), d = s.getDataRange().getValues();
  if (d.length<=1) return {success:true, orders:[]};
  const orders = [];
  for(let i=1; i<d.length; i++) {
    const r=d[i]; if(!r[0]) continue;
    orders.push({
      id:r[0], name:r[1], phone:r[2], store:r[3],
      street:r[4], details:r[5], status:r[6],
      driver:r[7]||'', time:r[8]||'',
      pickedTime:r[9]||'', deliveredTime:r[10]||'',
      lat:r[11]||'', lng:r[12]||''
    });
  }
  return {success:true, orders};
}

// إضافة طلب جديد + توزيع تلقائي
function addOrder(p) {
  const s   = getSheet();
  const id  = getNextId();
  const t   = Utilities.formatDate(new Date(),'Asia/Riyadh','yyyy/MM/dd HH:mm');
  s.appendRow([id,p.name,p.phone,p.store,p.street,p.details,'pending','',t,'','',p.lat||'',p.lng||'']);
  return {success:true, id};
}

// تحديث الحالة — مع التوزيع التلقائي عند اعتماد المحل
function updateStatus(p) {
  const s   = getSheet();
  const d   = s.getDataRange().getValues();
  const id  = parseInt(p.id);
  const now = Utilities.formatDate(new Date(),'Asia/Riyadh','HH:mm');

  for(let i=1; i<d.length; i++) {
    if(parseInt(d[i][0])===id) {
      const newStatus = p.status;
      s.getRange(i+1,7).setValue(newStatus);
      if(newStatus==='picked')     s.getRange(i+1,10).setValue(now);
      if(newStatus==='delivered')  {
        s.getRange(i+1,11).setValue(now);
        // بعد التسليم — وزّع أول طلب منتظر على هذا الموصل
        const thisDriver = d[i][7];
        autoAssignNext(thisDriver);
      }
      // عند اعتماد المحل — وزّع تلقائياً
      if(newStatus==='approved') {
        const assigned = autoAssignOrder(id);
        if(!assigned) {
          // لو ما في موصل متاح — الطلب ينتظر
          s.getRange(i+1,7).setValue('waiting');
        }
      }
      return {success:true};
    }
  }
  return {success:false, error:'not found'};
}

// التوزيع التلقائي — يبحث عن موصل متاح
function autoAssignOrder(orderId) {
  const s    = getSheet();
  const data = s.getDataRange().getValues();

  // أي موصل متاح (ما عنده dispatched أو picked)
  const busyDrivers = new Set();
  data.slice(1).forEach(r => {
    if(r[7] && (r[6]==='dispatched'||r[6]==='picked')) {
      busyDrivers.add(r[7]);
    }
  });

  // أقل موصل من حيث عدد طلباته اليوم
  const driverCounts = {};
  DRIVERS.forEach(d => driverCounts[d]=0);
  data.slice(1).forEach(r => {
    if(r[7] && DRIVERS.includes(r[7])) driverCounts[r[7]]++;
  });

  // اختر الموصل المتاح بأقل طلبات
  let chosen = null;
  let minCount = Infinity;
  DRIVERS.forEach(d => {
    if(!busyDrivers.has(d) && driverCounts[d]<minCount) {
      minCount = driverCounts[d];
      chosen = d;
    }
  });

  if(!chosen) return false;

  // عيّن الموصل للطلب
  for(let i=1; i<data.length; i++) {
    if(parseInt(data[i][0])===orderId) {
      s.getRange(i+1,7).setValue('dispatched');
      s.getRange(i+1,8).setValue(chosen);
      return true;
    }
  }
  return false;
}

// بعد تسليم طلب — وزّع أول طلب منتظر
function autoAssignNext(driverName) {
  const s    = getSheet();
  const data = s.getDataRange().getValues();

  for(let i=1; i<data.length; i++) {
    if(data[i][6]==='waiting') {
      s.getRange(i+1,7).setValue('dispatched');
      s.getRange(i+1,8).setValue(driverName);
      return;
    }
  }
}

// الموصل يعلن جاهزيته (اختياري)
function driverReady(p) {
  autoAssignNext(p.driver);
  return {success:true};
}

function getSheet() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  let sh   = ss.getSheetByName(SHEET_NAME);
  if(!sh) {
    sh = ss.insertSheet(SHEET_NAME);
    sh.appendRow(['ID','الاسم','الجوال','المحل','الشارع','الطلب','الحالة','الموصل','وقت الطلب','وقت الاستلام','وقت التسليم','Lat','Lng']);
    sh.setRightToLeft(true);
    sh.getRange(1,1,1,13).setFontWeight('bold').setBackground('#7c3aed').setFontColor('#fff');
    sh.setFrozenRows(1);
  }
  return sh;
}

function getNextId() {
  const sh = getSheet(), last = sh.getLastRow();
  if(last<=1) return 1001;
  const d  = sh.getRange(2,1,last-1,1).getValues();
  let max  = 1000;
  d.forEach(r=>{if(r[0]>max)max=r[0];});
  return max+1;
}
