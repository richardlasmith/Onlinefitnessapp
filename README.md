import React, { useState, useMemo } from 'react';

// Main App Component
function App() {
  const [currentView, setCurrentView] = useState('membership'); // 'membership', 'calendar', 'history'

  // Mock data for Membership Options
  const membershipOptions = useMemo(() => [
    {
      tier: 'Elite Solo',
      description: 'Individual Training for highly personalized attention.',
      options: [
        { sessions: '1 Session Weekly (4/month)', price: '$360 - $480 USD' },
        { sessions: '2 Sessions Weekly (8/month)', price: '$720 - $960 USD' },
        { sessions: 'Unlimited Sessions', price: '$1,200 - $1,800+ USD' },
      ],
    },
    {
      tier: 'Dynamic Duo',
      description: 'Couples Training for two people training together.',
      options: [
        { sessions: '1 Session Weekly (4/month)', price: '$480 - $720 USD (Total for two)' },
        { sessions: '2 Sessions Weekly (8/month)', price: '$960 - $1,440 USD (Total for two)' },
        { sessions: 'Unlimited Sessions', price: '$1,800 - $2,500+ USD (Total for two)' },
      ],
    },
    {
      tier: 'Synergy Squad',
      description: 'Small Group Training (4+ individuals) for a motivating and cost-effective experience.',
      options: [
        { sessions: '1 Session Weekly (4/month)', price: '$140 - $240 USD (per person)' },
        { sessions: '2 Sessions Weekly (8/month)', price: '$280 - $480 USD (per person)' },
        { sessions: 'Unlimited Sessions', price: '$500 - $800+ USD (per person)' },
      ],
    },
  ], []);

  // Mock data for Calendar (simple array of dates)
  const calendarDates = useMemo(() => {
    const today = new Date();
    const dates = [];
    for (let i = 0; i < 30; i++) {
      const date = new Date(today);
      date.setDate(today.getDate() + i);
      dates.push(date);
    }
    return dates;
  }, []);

  // Mock data for Check-in History
  const checkInHistory = useMemo(() => [
    { date: '2025-06-25', session: 'Elite Solo - Arms & Core' },
    { date: '2025-06-22', session: 'Dynamic Duo - Full Body Strength' },
    { date: '2025-06-19', session: 'Synergy Squad - HIIT' },
    { date: '2025-06-17', session: 'Elite Solo - Leg Day' },
    { date: '2025-06-15', session: 'Dynamic Duo - Cardio Blast' },
  ], []);

  // Conditional rendering based on currentView state
  const renderContent = () => {
    switch (currentView) {
      case 'membership':
        return <MembershipDisplay options={membershipOptions} />;
      case 'calendar':
        return <CalendarView dates={calendarDates} />;
      case 'history':
        return <CheckInHistory history={checkInHistory} />;
      default:
        return <MembershipDisplay options={membershipOptions} />;
    }
  };

  return (
    <div className="min-h-screen bg-gray-100 p-4 font-sans flex flex-col items-center">
      <header className="w-full max-w-4xl bg-white shadow-md rounded-lg p-6 mb-8">
        <h1 className="text-4xl font-bold text-center text-indigo-700 mb-6">
          \[Your Studio Name] Fitness App
        </h1>
        <nav className="flex flex-wrap justify-center gap-4">
          <Button onClick={() => setCurrentView('membership')} isActive={currentView === 'membership'}>
            Membership Options
          </Button>
          <Button onClick={() => setCurrentView('calendar')} isActive={currentView === 'calendar'}>
            Calendar
          </Button>
          <Button onClick={() => setCurrentView('history')} isActive={currentView === 'history'}>
            Check-in History
          </Button>
        </nav>
      </header>

      <main className="w-full max-w-4xl bg-white shadow-md rounded-lg p-6">
        {renderContent()}
      </main>

      <footer className="w-full max-w-4xl text-center text-gray-600 text-sm mt-8 p-4">
        &copy; {new Date().getFullYear()} \[Your Studio Name]. All rights reserved.
      </footer>
    </div>
  );
}

// Reusable Button Component
const Button = ({ children, onClick, isActive }) => (
  <button
    onClick={onClick}
    className={`px-6 py-3 rounded-xl font-semibold transition-all duration-300 ease-in-out
      ${isActive
        ? 'bg-indigo-600 text-white shadow-lg hover:bg-indigo-700'
        : 'bg-gray-200 text-gray-800 hover:bg-gray-300'
      }
      focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-75`}
  >
    {children}
  </button>
);

// Membership Options Display Component
const MembershipDisplay = ({ options }) => (
  <section className="text-gray-800">
    <h2 className="text-3xl font-semibold text-center text-indigo-700 mb-8">Membership Options</h2>
    <div className="grid md:grid-cols-3 gap-6">
      {options.map((tier, index) => (
        <div key={index} className="bg-gradient-to-br from-indigo-50 to-purple-50 p-6 rounded-xl shadow-md border border-indigo-200 hover:shadow-lg transform hover:scale-105 transition-all duration-300">
          <h3 className="text-2xl font-bold text-indigo-800 mb-4 text-center">{tier.tier}</h3>
          <p className="text-center text-gray-600 mb-6 italic">{tier.description}</p>
          <ul className="space-y-4">
            {tier.options.map((option, idx) => (
              <li key={idx} className="flex flex-col items-center bg-white p-4 rounded-lg shadow-sm border border-gray-200">
                <span className="font-semibold text-lg text-gray-700">{option.sessions}</span>
                <span className="text-indigo-600 font-bold text-xl mt-1">{option.price}</span>
              </li>
            ))}
          </ul>
        </div>
      ))}
    </div>
    <div className="mt-8 text-center text-gray-600">
        <p className="text-lg">All memberships are subject to a 12-month commitment. Rates based on Charlotte, NC area. Fair use policy applies to Unlimited Sessions.</p>
    </div>
  </section>
);

// Calendar View Component
const CalendarView = ({ dates }) => {
  const [currentMonth, setCurrentMonth] = useState(new Date());

  const getDaysInMonth = (year, month) => {
    return new Date(year, month + 1, 0).getDate();
  };

  const getFirstDayOfMonth = (year, month) => {
    return new Date(year, month, 1).getDay(); // 0 for Sunday, 1 for Monday, etc.
  };

  const displayMonth = currentMonth.toLocaleString('en-US', { month: 'long', year: 'numeric' });
  const daysInMonth = getDaysInMonth(currentMonth.getFullYear(), currentMonth.getMonth());
  const firstDayIndex = getFirstDayOfMonth(currentMonth.getFullYear(), currentMonth.getMonth());

  const prevMonth = () => {
    setCurrentMonth(prev => new Date(prev.getFullYear(), prev.getMonth() - 1, 1));
  };

  const nextMonth = () => {
    setCurrentMonth(prev => new Date(prev.getFullYear(), prev.getMonth() + 1, 1));
  };

  const dayNames = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

  // Mock available times for demonstration
  const availableTimes = ['9:00 AM', '10:30 AM', '1:00 PM', '3:00 PM', '5:30 PM'];

  return (
    <section className="text-gray-800">
      <h2 className="text-3xl font-semibold text-center text-indigo-700 mb-8">Calendar</h2>
      <div className="flex justify-between items-center mb-6 px-4">
        <button onClick={prevMonth} className="p-2 rounded-full bg-gray-200 hover:bg-gray-300 transition-colors">
          <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6 text-gray-700" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M15 19l-7-7 7-7" />
          </svg>
        </button>
        <h3 className="text-2xl font-bold text-indigo-800">{displayMonth}</h3>
        <button onClick={nextMonth} className="p-2 rounded-full bg-gray-200 hover:bg-gray-300 transition-colors">
          <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6 text-gray-700" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 5l7 7-7 7" />
          </svg>
        </button>
      </div>

      <div className="grid grid-cols-7 text-center font-bold text-gray-600 mb-2">
        {dayNames.map(day => <div key={day} className="py-2">{day}</div>)}
      </div>

      <div className="grid grid-cols-7 gap-2">
        {/* Empty cells for leading days */}
        {Array.from({ length: firstDayIndex }).map((_, i) => (
          <div key={`empty-${i}`} className="p-3 text-center bg-gray-50 rounded-lg"></div>
        ))}
        {/* Actual days of the month */}
        {Array.from({ length: daysInMonth }).map((_, i) => {
          const day = i + 1;
          const fullDate = new Date(currentMonth.getFullYear(), currentMonth.getMonth(), day);
          const isToday = fullDate.toDateString() === new Date().toDateString();
          const hasSessions = day % 2 === 0; // Mock: every other day has sessions

          return (
            <div
              key={day}
              className={`p-3 text-center rounded-lg cursor-pointer transition-all duration-200
                ${isToday ? 'bg-indigo-500 text-white font-bold shadow-md' : 'bg-gray-100 hover:bg-indigo-100'}
                ${hasSessions ? 'border border-indigo-300' : 'border border-gray-200'}`}
            >
              <div className="text-lg">{day}</div>
              {hasSessions && (
                <div className="text-xs text-indigo-700 mt-1">
                  {availableTimes.length} slots
                </div>
              )}
            </div>
          );
        })}
      </div>

      <div className="mt-8 p-4 bg-indigo-50 rounded-xl border border-indigo-200">
          <h4 className="text-xl font-semibold text-indigo-800 mb-4">Available Session Times (Sample)</h4>
          <ul className="grid grid-cols-2 sm:grid-cols-3 gap-3">
              {availableTimes.map((time, idx) => (
                  <li key={idx} className="bg-white p-2 rounded-lg text-center text-gray-700 font-medium shadow-sm">
                      {time}
                  </li>
              ))}
          </ul>
          <p className="text-sm text-gray-600 mt-4 italic">Please contact the studio to book your sessions.</p>
      </div>
    </section>
  );
};

// Check-in History Component
const CheckInHistory = ({ history }) => (
  <section className="text-gray-800">
    <h2 className="text-3xl font-semibold text-center text-indigo-700 mb-8">Check-in History</h2>
    {history.length > 0 ? (
      <div className="space-y-4">
        {history.map((record, index) => (
          <div key={index} className="bg-blue-50 p-4 rounded-xl shadow-sm border border-blue-200 flex flex-col sm:flex-row justify-between items-center">
            <span className="font-semibold text-xl text-blue-800 mb-2 sm:mb-0">
              {new Date(record.date).toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}
            </span>
            <span className="text-lg text-gray-700 text-center sm:text-right">
              {record.session}
            </span>
          </div>
        ))}
      </div>
    ) : (
      <p className="text-center text-gray-600 text-lg">No check-in history found yet. Start training today!</p>
    )}
  </section>
);

export default App;
