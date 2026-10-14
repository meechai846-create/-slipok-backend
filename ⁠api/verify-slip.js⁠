const axios = require('axios');
const FormData = require('form-data');

module.exports = async (req, res) => {
  if (req.method !== 'POST') {
    return res.status(405).json({ success: false, message: 'Method Not Allowed' });
  }

  try {
    const { imageData } = req.body;
    if (!imageData) {
      return res.status(400).json({ success: false, message: 'ไม่พบรูปภาพสลิป' });
    }

    const buffer = Buffer.from(imageData.split(',')[1], 'base64');
    const formData = new FormData();
    formData.append('files', buffer, 'slip.jpg');
    formData.append('log', 'true');
    formData.append('amount', '499');

    const response = await axios.post('https://api.slipok.com/api/line/apikey/78561', formData, {
      headers: {
        'x-authorization': process.env.SLIPOK_API_KEY,
        ...formData.getHeaders()
      }
    });

    const result = response.data;

    if (result.success) {
      return res.status(200).json({ 
        success: true, 
        message: 'ตรวจสอบสลิปสำเร็จ',
        data: result.data 
      });
    } else {
      return res.status(400).json({ 
        success: false, 
        message: result.message || 'สลิปไม่ถูกต้อง ยอดเงินไม่ตรง หรือเป็นสลิปซ้ำ' 
      });
    }

  } catch (error) {
    console.error(error);
    return res.status(500).json({ 
      success: false, 
      message: 'เกิดข้อผิดพลาดในการเชื่อมต่อกับระบบตรวจสอบสลิป' 
    });
  }
};
