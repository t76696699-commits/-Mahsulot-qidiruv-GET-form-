// userApi.js — tashqi API bilan ishlaydigan modul
async function fetchUser(id) {
  const response = await fetch(`https://api.example.com/users/${id}`);
  if (!response.ok) {
    throw new Error('Foydalanuvchi topilmadi');
  }
  return response.json();
}

function logEvent(logger, eventName) {
  logger.log(`Event: ${eventName}`);
  return true;
}

module.exports = { fetchUser, logEvent };

// userApi.test.js — mock va spy misollari
const { fetchUser, logEvent } = require('./userApi');

describe('fetchUser — mock bilan test', () => {
  beforeEach(() => {
    // Har bir testdan oldin global fetch'ni mocklaymiz
    global.fetch = jest.fn();
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  test('muvaffaqiyatli javobda foydalanuvchi qaytaradi', async () => {
    global.fetch.mockResolvedValue({
      ok: true,
      json: async () => ({ id: 1, name: 'Aziz' }),
    });

    const user = await fetchUser(1);

    expect(user).toEqual({ id: 1, name: 'Aziz' });
    expect(global.fetch).toHaveBeenCalledWith(
      'https://api.example.com/users/1'
    );
    expect(global.fetch).toHaveBeenCalledTimes(1);
  });

  test('xato javobda Error tashlaydi', async () => {
    global.fetch.mockResolvedValue({ ok: false });

    await expect(fetchUser(999)).rejects.toThrow(
      'Foydalanuvchi topilmadi'
    );
  });
});

describe('logEvent — spy bilan test', () => {
  test('logger.log to\'g\'ri argument bilan chaqiriladi', () => {
    const logger = { log: () => {} };
    const spy = jest.spyOn(logger, 'log');

    logEvent(logger, 'user_login');

    expect(spy).toHaveBeenCalledWith('Event: user_login');
    spy.mockRestore();
  });
});

// Ishga tushirish: npx jest userApi.test.js --verbose
