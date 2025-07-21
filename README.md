```
import React, { useState } from 'react';

export const TimeTabs = () => {
  const [activeTab, setActiveTab] = useState(0);
  const tabs = ['Секунды', 'Минуты', 'Часы', 'Дни', 'Месяцы', 'Годы'];

  // Состояния для вкладки "Секунды"
  const [everySeconds, setEverySeconds] = useState(true);
  const [secondsInterval, setSecondsInterval] = useState(1);
  const [secondsStartingAt, setSecondsStartingAt] = useState(0);
  const [selectedSeconds, setSelectedSeconds] = useState<number[]>([]);
  const [secondsRangeFrom, setSecondsRangeFrom] = useState(0);
  const [secondsRangeTo, setSecondsRangeTo] = useState(0);

  // Состояния для вкладки "Минуты"
  const [everyMinutes, setEveryMinutes] = useState(true);
  const [minutesInterval, setMinutesInterval] = useState(1);
  const [minutesStartingAt, setMinutesStartingAt] = useState(0);
  const [selectedMinutes, setSelectedMinutes] = useState<number[]>([]);
  const [minutesRangeFrom, setMinutesRangeFrom] = useState(0);
  const [minutesRangeTo, setMinutesRangeTo] = useState(0);

  // Состояния для вкладки "Часы"
  const [everyHours, setEveryHours] = useState(true);
  const [hoursInterval, setHoursInterval] = useState(1);
  const [hoursStartingAt, setHoursStartingAt] = useState(0);
  const [selectedHours, setSelectedHours] = useState<number[]>([]);
  const [hoursRangeFrom, setHoursRangeFrom] = useState(0);
  const [hoursRangeTo, setHoursRangeTo] = useState(0);

  // Состояния для вкладки "Дни"
  const [everyDays, setEveryDays] = useState(true);
  const [daysInterval, setDaysInterval] = useState(1);
  const [startDayOfWeek, setStartDayOfWeek] = useState('Sunday');
  const [startDayOfMonth, setStartDayOfMonth] = useState(1);
  const [selectedDaysOfWeek, setSelectedDaysOfWeek] = useState<string[]>([]);
  const [selectedDaysOfMonth, setSelectedDaysOfMonth] = useState<number[]>([]);
  const [lastDayOptions, setLastDayOptions] = useState({
    lastDay: false,
    lastWeekday: false,
    lastSpecificDay: '',
    daysBeforeEnd: 0,
    nearestWeekday: false,
    firstSpecificDay: ''
  });

  // Состояния для вкладки "Месяцы"
  const [everyMonths, setEveryMonths] = useState(true);
  const [monthsInterval, setMonthsInterval] = useState(1);
  const [startingMonth, setStartingMonth] = useState('JAN');
  const [selectedMonths, setSelectedMonths] = useState<string[]>([]);
  const [rangeFromMonth, setRangeFromMonth] = useState('JAN');
  const [rangeToMonth, setRangeToMonth] = useState('DEC');

  // Состояния для вкладки "Годы"
  const [everyYears, setEveryYears] = useState(true);
  const [yearsInterval, setYearsInterval] = useState(1);
  const [startingYear, setStartingYear] = useState(2022);
  const [selectedYears, setSelectedYears] = useState<number[]>([]);
  const [rangeFromYear, setRangeFromYear] = useState(2022);
  const [rangeToYear, setRangeToYear] = useState(2022);

  // Вспомогательные данные
  const daysOfWeek = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
  
  const months = [
    { value: 'JAN', label: 'JAN' }, { value: 'FEB', label: 'FEB' },
    { value: 'MAR', label: 'MAR' }, { value: 'APR', label: 'APR' },
    { value: 'MAY', label: 'MAY' }, { value: 'JUN', label: 'JUN' },
    { value: 'JUL', label: 'JUL' }, { value: 'AUG', label: 'AUG' },
    { value: 'SEP', label: 'SEP' }, { value: 'OCT', label: 'OCT' },
    { value: 'NOV', label: 'NOV' }, { value: 'DEC', label: 'DEC' }
  ];

  const years = Array.from({ length: 80 }, (_, i) => 2020 + i);

  // Обработчики событий
  const toggleSecond = (second: number) => {
    setSelectedSeconds(prev => 
      prev.includes(second) ? prev.filter(s => s !== second) : [...prev, second]
    );
  };

  const toggleMinute = (minute: number) => {
    setSelectedMinutes(prev => 
      prev.includes(minute) ? prev.filter(m => m !== minute) : [...prev, minute]
    );
  };

  const toggleHour = (hour: number) => {
    setSelectedHours(prev => 
      prev.includes(hour) ? prev.filter(h => h !== hour) : [...prev, hour]
    );
  };

  const toggleDayOfWeek = (day: string) => {
    setSelectedDaysOfWeek(prev => 
      prev.includes(day) ? prev.filter(d => d !== day) : [...prev, day]
    );
  };

  const toggleDayOfMonth = (day: number) => {
    setSelectedDaysOfMonth(prev => 
      prev.includes(day) ? prev.filter(d => d !== day) : [...prev, day]
    );
  };

  const toggleMonth = (month: string) => {
    setSelectedMonths(prev => 
      prev.includes(month) ? prev.filter(m => m !== month) : [...prev, month]
    );
  };

  const toggleYear = (year: number) => {
    setSelectedYears(prev => 
      prev.includes(year) ? prev.filter(y => y !== year) : [...prev, year]
    );
  };

  const handleLastDayOption = (option: string) => {
    setLastDayOptions(prev => ({ ...prev, [option]: !prev[option] }));
  };

  return (
    <div style={{ fontFamily: 'Arial, sans-serif', maxWidth: '800px', margin: '0 auto' }}>
      <div style={{ display: 'flex', borderBottom: '1px solid #ccc' }}>
        {tabs.map((tab, index) => (
          <button
            key={index}
            style={{
              padding: '10px 20px',
              cursor: 'pointer',
              backgroundColor: activeTab === index ? '#f0f0f0' : 'white',
              border: 'none',
              borderBottom: activeTab === index ? '2px solid #007bff' : '1px solid #ccc',
              marginRight: '5px',
              borderRadius: '5px 5px 0 0',
              outline: 'none',
              fontSize: '14px',
            }}
            onClick={() => setActiveTab(index)}
          >
            {tab}
          </button>
        ))}
      </div>
      
      <div style={{ padding: '20px', border: '1px solid #ccc', borderTop: 'none', minHeight: '200px' }}>
        {/* Вкладка Секунды */}
        {/* {activeTab === 0 && (
          <div>
            <h3>Every second</h3>
            <div>
              <input 
                type="checkbox" 
                checked={everySeconds} 
                onChange={() => setEverySeconds(!everySeconds)} 
              /> Every
              <input 
                type="number" 
                value={secondsInterval} 
                onChange={(e) => setSecondsInterval(Math.max(1, parseInt(e.target.value) || 1)} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              /> second starting at second
              <input 
                type="number" 
                value={secondsStartingAt} 
                onChange={(e) => setSecondsStartingAt(Math.min(59, Math.max(0, parseInt(e.target.value) || 0)))} 
                min="0"
                max="59"
                style={{ width: '50px', margin: '0 5px' }}
              />
            </div>

            <hr />

            <h4>Specific second (choose one or many)</h4>
            <div style={{ display: 'flex', flexWrap: 'wrap' }}>
              {Array.from({ length: 60 }, (_, i) => i).map(second => (
                <div key={second} style={{ margin: '5px', width: '40px' }}>
                  <input
                    type="checkbox"
                    id={`second-${second}`}
                    checked={selectedSeconds.includes(second)}
                    onChange={() => toggleSecond(second)}
                  />
                  <label htmlFor={`second-${second}`}>
                    {second.toString().padStart(2, '0')}
                  </label>
                </div>
              ))}
            </div>

            <hr />

            <div>
              <input 
                type="checkbox" 
                checked={!everySeconds && selectedSeconds.length === 0} 
                onChange={() => {
                  setEverySeconds(false);
                  setSelectedSeconds([]);
                }} 
              /> Every second between second
              <input 
                type="number" 
                value={secondsRangeFrom} 
                onChange={(e) => setSecondsRangeFrom(Math.min(59, Math.max(0, parseInt(e.target.value) || 0)))} 
                min="0"
                max="59"
                style={{ width: '50px', margin: '0 5px' }}
              /> and second
              <input 
                type="number" 
                value={secondsRangeTo} 
                onChange={(e) => setSecondsRangeTo(Math.min(59, Math.max(0, parseInt(e.target.value) || 0)))} 
                min="0"
                max="59"
                style={{ width: '50px', margin: '0 5px' }}
              />
            </div>
          </div>
        )} */}

        {/* Вкладка Минуты */}
        {activeTab === 1 && (
          <div>
            <h3>Every minute</h3>
            <div>
              <input 
                type="checkbox" 
                checked={everyMinutes} 
                onChange={() => setEveryMinutes(!everyMinutes)} 
              /> Every
              <input 
                type="number" 
                value={minutesInterval} 
                onChange={(e) => setMinutesInterval(Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              /> minute(s) starting at minute
              <input 
                type="number" 
                value={minutesStartingAt} 
                onChange={(e) => setMinutesStartingAt(Math.min(59, Math.max(0, parseInt(e.target.value) || 0))} 
                min="0"
                max="59"
                style={{ width: '50px', margin: '0 5px' }}
              />
            </div>

            <hr />

            <h4>Specific minute (choose one or many)</h4>
            <div style={{ display: 'flex', flexWrap: 'wrap' }}>
              {Array.from({ length: 60 }, (_, i) => i).map(minute => (
                <div key={minute} style={{ margin: '5px', width: '40px' }}>
                  <input
                    type="checkbox"
                    id={`minute-${minute}`}
                    checked={selectedMinutes.includes(minute)}
                    onChange={() => toggleMinute(minute)}
                  />
                  <label htmlFor={`minute-${minute}`}>
                    {minute.toString().padStart(2, '0')}
                  </label>
                </div>
              ))}
            </div>

            <hr />

            <div>
              <input 
                type="checkbox" 
                checked={!everyMinutes && selectedMinutes.length === 0} 
                onChange={() => {
                  setEveryMinutes(false);
                  setSelectedMinutes([]);
                }} 
              /> Every minute between minute
              <input 
                type="number" 
                value={minutesRangeFrom} 
                onChange={(e) => setMinutesRangeFrom(Math.min(59, Math.max(0, parseInt(e.target.value) || 0))} 
                min="0"
                max="59"
                style={{ width: '50px', margin: '0 5px' }}
              /> and minute
              <input 
                type="number" 
                value={minutesRangeTo} 
                onChange={(e) => setMinutesRangeTo(Math.min(59, Math.max(0, parseInt(e.target.value) || 0))} 
                min="0"
                max="59"
                style={{ width: '50px', margin: '0 5px' }}
              />
            </div>
          </div>
        )}

        {/* Вкладка Часы */}
        {activeTab === 2 && (
          <div>
            <h3>Every hour</h3>
            <div>
              <input 
                type="checkbox" 
                checked={everyHours} 
                onChange={() => setEveryHours(!everyHours)} 
              /> Every
              <input 
                type="number" 
                value={hoursInterval} 
                onChange={(e) => setHoursInterval(Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              /> hour(s) starting at hour
              <input 
                type="number" 
                value={hoursStartingAt} 
                onChange={(e) => setHoursStartingAt(Math.min(23, Math.max(0, parseInt(e.target.value) || 0))} 
                min="0"
                max="23"
                style={{ width: '50px', margin: '0 5px' }}
              />
            </div>

            <hr />

            <h4>Specific hour (choose one or many)</h4>
            <div style={{ display: 'flex', flexWrap: 'wrap' }}>
              {Array.from({ length: 24 }, (_, i) => i).map(hour => (
                <div key={hour} style={{ margin: '5px', width: '40px' }}>
                  <input
                    type="checkbox"
                    id={`hour-${hour}`}
                    checked={selectedHours.includes(hour)}
                    onChange={() => toggleHour(hour)}
                  />
                  <label htmlFor={`hour-${hour}`}>
                    {hour.toString().padStart(2, '0')}
                  </label>
                </div>
              ))}
            </div>

            <hr />

            <div>
              <input 
                type="checkbox" 
                checked={!everyHours && selectedHours.length === 0} 
                onChange={() => {
                  setEveryHours(false);
                  setSelectedHours([]);
                }} 
              /> Every hour between hour
              <input 
                type="number" 
                value={hoursRangeFrom} 
                onChange={(e) => setHoursRangeFrom(Math.min(23, Math.max(0, parseInt(e.target.value) || 0)))} 
                min="0"
                max="23"
                style={{ width: '50px', margin: '0 5px' }}
              /> and hour
              <input 
                type="number" 
                value={hoursRangeTo} 
                onChange={(e) => setHoursRangeTo(Math.min(23, Math.max(0, parseInt(e.target.value) || 0)))} 
                min="0"
                max="23"
                style={{ width: '50px', margin: '0 5px' }}
              />
            </div>
          </div>
        )}

        {/* Вкладка Дни */}
        {activeTab === 3 && (
          <div>
            <h3>Every day</h3>
            
            <div style={{ marginBottom: '15px' }}>
              <input 
                type="checkbox" 
                checked={everyDays} 
                onChange={() => setEveryDays(!everyDays)} 
              /> Every 
              <input 
                type="number" 
                value={daysInterval} 
                onChange={(e) => setDaysInterval(Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              /> day(s) starting on
              <select 
                value={startDayOfWeek} 
                onChange={(e) => setStartDayOfWeek(e.target.value)}
                style={{ margin: '0 5px' }}
              >
                {daysOfWeek.map(day => (
                  <option key={day} value={day}>{day}</option>
                ))}
              </select>
            </div>

            <div style={{ marginBottom: '15px' }}>
              <input 
                type="checkbox" 
                checked={!everyDays} 
                onChange={() => setEveryDays(false)} 
              /> Every 
              <input 
                type="number" 
                value={daysInterval} 
                onChange={(e) => setDaysInterval(Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              /> day(s) starting on the 
              <input 
                type="number" 
                value={startDayOfMonth} 
                onChange={(e) => setStartDayOfMonth(Math.min(31, Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                max="31"
                style={{ width: '50px', margin: '0 5px' }}
              /> of the month
            </div>

            <hr />

            <h4>Specific day of the week (choose one or many)</h4>
            <div style={{ display: 'flex', flexWrap: 'wrap', marginBottom: '15px' }}>
              {daysOfWeek.map(day => (
                <div key={day} style={{ margin: '5px', width: '100px' }}>
                  <input
                    type="checkbox"
                    id={`weekday-${day}`}
                    checked={selectedDaysOfWeek.includes(day)}
                    onChange={() => toggleDayOfWeek(day)}
                  />
                  <label htmlFor={`weekday-${day}`}>{day}</label>
                </div>
              ))}
            </div>

            <h4>Specific day of month (choose one or many)</h4>
            <div style={{ display: 'flex', flexWrap: 'wrap', marginBottom: '15px' }}>
              {Array.from({ length: 31 }, (_, i) => i + 1).map(day => (
                <div key={day} style={{ margin: '5px', width: '40px' }}>
                  <input
                    type="checkbox"
                    id={`monthday-${day}`}
                    checked={selectedDaysOfMonth.includes(day)}
                    onChange={() => toggleDayOfMonth(day)}
                  />
                  <label htmlFor={`monthday-${day}`}>{day.toString().padStart(2, '0')}</label>
                </div>
              ))}
            </div>

            <hr />

            <div style={{ marginBottom: '10px' }}>
              <input 
                type="checkbox" 
                checked={lastDayOptions.lastDay} 
                onChange={() => handleLastDayOption('lastDay')} 
              /> On the last day of the month
            </div>

            <div style={{ marginBottom: '10px' }}>
              <input 
                type="checkbox" 
                checked={lastDayOptions.lastWeekday} 
                onChange={() => handleLastDayOption('lastWeekday')} 
              /> On the last weekday of the month
            </div>

            <div style={{ marginBottom: '10px' }}>
              <input 
                type="checkbox" 
                checked={lastDayOptions.lastSpecificDay !== ''} 
                onChange={() => handleLastDayOption('lastSpecificDay')} 
              /> On the last 
              <select 
                value={lastDayOptions.lastSpecificDay} 
                onChange={(e) => setLastDayOptions({...lastDayOptions, lastSpecificDay: e.target.value})}
                style={{ margin: '0 5px' }}
              >
                <option value="">Select day</option>
                {daysOfWeek.map(day => (
                  <option key={day} value={day}>{day}</option>
                ))}
              </select> of the month
            </div>

            <div style={{ marginBottom: '10px' }}>
              <input 
                type="checkbox" 
                checked={lastDayOptions.daysBeforeEnd > 0} 
                onChange={() => setLastDayOptions({...lastDayOptions, daysBeforeEnd: lastDayOptions.daysBeforeEnd > 0 ? 0 : 1})} 
              /> On the last 
              <input 
                type="number" 
                value={lastDayOptions.daysBeforeEnd} 
                onChange={(e) => setLastDayOptions({...lastDayOptions, daysBeforeEnd: Math.min(31, Math.max(0, parseInt(e.target.value) || 0))})} 
                min="1"
                max="31"
                style={{ width: '50px', margin: '0 5px' }}
              /> day(s) before the end of the month
            </div>

            <div style={{ marginBottom: '10px' }}>
              <input 
                type="checkbox" 
                checked={lastDayOptions.nearestWeekday} 
                onChange={() => handleLastDayOption('nearestWeekday')} 
              /> Nearest weekday (Monday to Friday) to the 
              <input 
                type="number" 
                value={startDayOfMonth} 
                onChange={(e) => setStartDayOfMonth(Math.min(31, Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                max="31"
                style={{ width: '50px', margin: '0 5px' }}
              /> of the month
            </div>

            <div>
              <input 
                type="checkbox" 
                checked={lastDayOptions.firstSpecificDay !== ''} 
                onChange={() => handleLastDayOption('firstSpecificDay')} 
              /> On the 
              <input 
                type="number" 
                value={startDayOfMonth} 
                onChange={(e) => setStartDayOfMonth(Math.min(31, Math.max(1, parseInt(e.target.value) || 1)))} 
                min="1"
                max="31"
                style={{ width: '50px', margin: '0 5px' }}
              /> 
              <select 
                value={lastDayOptions.firstSpecificDay} 
                onChange={(e) => setLastDayOptions({...lastDayOptions, firstSpecificDay: e.target.value})}
                style={{ margin: '0 5px' }}
              >
                <option value="">Select day</option>
                {daysOfWeek.map(day => (
                  <option key={day} value={day}>{day}</option>
                ))}
              </select> of the month
            </div>
          </div>
        )}

        {/* Вкладка Месяцы */}
        {activeTab === 4 && (
          <div>
            <h3>Every month</h3>
            
            <div style={{ marginBottom: '15px', display: 'flex', alignItems: 'center' }}>
              <input 
                type="radio" 
                checked={everyMonths}
                onChange={() => setEveryMonths(true)}
                style={{ marginRight: '10px' }}
              />
              Every
              <input 
                type="number" 
                value={monthsInterval} 
                onChange={(e) => setMonthsInterval(Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              />
              month(s) starting in
              <select 
                value={startingMonth} 
                onChange={(e) => setStartingMonth(e.target.value)}
                style={{ marginLeft: '5px' }}
              >
                {months.map(month => (
                  <option key={month.value} value={month.value}>{month.label}</option>
                ))}
              </select>
            </div>

            <div style={{ marginBottom: '15px' }}>
              <input 
                type="radio" 
                checked={!everyMonths && selectedMonths.length > 0}
                onChange={() => {
                  setEveryMonths(false);
                  setSelectedMonths([]);
                }}
                style={{ marginRight: '10px' }}
              />
              Specific month (choose one or many)
            </div>
            
            <div style={{ display: 'flex', flexWrap: 'wrap', marginBottom: '15px' }}>
              {months.map(month => (
                <div key={month.value} style={{ margin: '5px', width: '60px' }}>
                  <input
                    type="checkbox"
                    id={`month-${month.value}`}
                    checked={selectedMonths.includes(month.value)}
                    onChange={() => toggleMonth(month.value)}
                    disabled={everyMonths}
                    style={{ marginRight: '5px' }}
                  />
                  <label htmlFor={`month-${month.value}`}>{month.label}</label>
                </div>
              ))}
            </div>

            <div style={{ display: 'flex', alignItems: 'center' }}>
              <input 
                type="radio" 
                checked={!everyMonths && selectedMonths.length === 0}
                onChange={() => {
                  setEveryMonths(false);
                  setSelectedMonths([]);
                }}
                style={{ marginRight: '10px' }}
              />
              Every month between
              <select 
                value={rangeFromMonth} 
                onChange={(e) => setRangeFromMonth(e.target.value)}
                style={{ margin: '0 5px' }}
              >
                {months.map(month => (
                  <option key={month.value} value={month.value}>{month.label}</option>
                ))}
              </select>
              and
              <select 
                value={rangeToMonth} 
                onChange={(e) => setRangeToMonth(e.target.value)}
                style={{ marginLeft: '5px' }}
              >
                {months.map(month => (
                  <option key={month.value} value={month.value}>{month.label}</option>
                ))}
              </select>
            </div>
          </div>
        )}

        {/* Вкладка Годы */}
        {activeTab === 5 && (
          <div>
            <h3>Any year</h3>
            
            <div style={{ marginBottom: '15px' }}>
              <input 
                type="checkbox" 
                checked={everyYears} 
                onChange={() => setEveryYears(!everyYears)} 
              /> Every
              <input 
                type="number" 
                value={yearsInterval} 
                onChange={(e) => setYearsInterval(Math.max(1, parseInt(e.target.value) || 1))} 
                min="1"
                style={{ width: '50px', margin: '0 5px' }}
              /> year(s) starting in
              <input 
                type="number" 
                value={startingYear} 
                onChange={(e) => setStartingYear(Math.min(2099, Math.max(2020, parseInt(e.target.value) || 2022))} 
                min="2020"
                max="2099"
                style={{ width: '70px', margin: '0 5px' }}
              />
            </div>

            <h4>Specific year (choose one or many)</h4>
            <div style={{ display: 'grid', gridTemplateColumns: 'repeat(5, 1fr)', gap: '10px', marginBottom: '15px' }}>
              {years.map(year => (
                <div key={year} style={{ display: 'flex', alignItems: 'center' }}>
                  <input
                    type="checkbox"
                    id={`year-${year}`}
                    checked={selectedYears.includes(year)}
                    onChange={() => toggleYear(year)}
                    disabled={everyYears}
                    style={{ marginRight: '5px' }}
                  />
                  <label htmlFor={`year-${year}`}>{year}</label>
                </div>
              ))}
            </div>

            <div style={{ display: 'flex', alignItems: 'center' }}>
              <input 
                type="checkbox" 
                checked={!everyYears && selectedYears.length === 0} 
                onChange={() => {
                  setEveryYears(false);
                  setSelectedYears([]);
                }} 
              /> Every year between
              <input 
                type="number" 
                value={rangeFromYear} 
                onChange={(e) => setRangeFromYear(Math.min(2099, Math.max(2020, parseInt(e.target.value) || 2022)))} 
                min="2020"
                max="2099"
                style={{ width: '70px', margin: '0 5px' }}
              /> and
              <input 
                type="number" 
                value={rangeToYear} 
                onChange={(e) => setRangeToYear(Math.min(2099, Math.max(2020, parseInt(e.target.value) || 2022)))} 
                min="2020"
                max="2099"
                style={{ width: '70px', margin: '0 5px' }}
              />
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

```
