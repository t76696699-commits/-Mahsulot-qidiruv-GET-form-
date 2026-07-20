// weatherService.js — asinxron funksiyalar
function getTemperature(city) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (city === 'Toshkent') {
        resolve(28);
      } else {
        reject(new Error("Shahar topilmadi"));
      }
    }, 100);
  });
}

async function getTemperatureAsync(city) {
  const temp = await getTemperature(city);
  return `${city}: ${temp}°C`;
}

function fetchWithCallback(city, callback) {
  setTimeout(() => {
    if (city === 'Toshkent') {
      callback(null, 28);
    } else {
      callback(new Error('Shahar topilmadi'));
    }
  }, 100);
}

module.exports = { getTemperature, getTemperatureAsync, fetchWithCallback };

// weatherService.test.js — asinxron test usullari
const {
  getTemperature,
  getTemperatureAsync,
  fetchWithCallback,
} = require('./weatherService');

describe('async/await uslubi', () => {
  test('Toshkent uchun harorat qaytaradi', async () => {
    const result = await getTemperatureAsync('Toshkent');
    expect(result).toBe('Toshkent: 28°C');
  });

  test('noma\'lum shahar uchun xato tashlaydi', async () => {
    await expect(getTemperature('Marsx')).rejects.toThrow(
      'Shahar topilmadi'
    );
  });
});

describe('Promise qaytarish uslubi', () => {
  test('harorat 28 ga teng', () => {
    return getTemperature('Toshkent').then((temp) => {
      expect(temp).toBe(28);
    });
  });
});

describe('resolves/rejects uslubi', () => {
  test('resolves orqali tekshirish', async () => {
    await expect(getTemperature('Toshkent')).resolves.toBe(28);
  });

  test('rejects orqali tekshirish', async () => {
    await expect(getTemperature('Noma\'lum')).rejects.toThrow();
  });
});

describe('callback uslubi', () => {
  test('done bilan callback natijasini tekshirish', (done) => {
    fetchWithCallback('Toshkent', (err, temp) => {
      expect(err).toBeNull();
      expect(temp).toBe(28);
      done();
    });
  });
});

// Ishga tushirish: npx jest weatherService.test.js --verbose
