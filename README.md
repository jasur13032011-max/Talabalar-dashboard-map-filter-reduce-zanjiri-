# Talabalar-dashboard-map-filter-reduce-zanjiri-
const students = [
  { id: 1, name: "Anvar", age: 20, major: "Frontend", score: 85, tags: ["active", "mentor"] },
  { id: 2, name: "Diyora", age: 19, major: "Design", score: 92, tags: ["creative", "top"] },
  { id: 3, name: "Bekzod", age: 22, major: "Backend", score: 74, tags: ["hardworking"] },
  { id: 4, name: "Laylo", age: 21, major: "Frontend", score: 88, tags: ["active", "top"] },
  { id: 5, name: "Sardor", age: 23, major: "Data Science", score: 65, tags: ["researcher"] },
  { id: 6, name: "Madina", age: 18, major: "Design", score: 95, tags: ["top", "young"] },
  { id: 7, name: "Jasur", age: 24, major: "Backend", score: 50, tags: ["borderline"] },
  { id: 8, name: "Guli", age: 20, major: "Frontend", score: 79, tags: ["hardworking"] },
  { id: 9, name: "Farruh", age: 22, major: "Data Science", score: 91, tags: ["top", "mentor"] },
  { id: 10, name: "Zuhra", age: 19, major: "Mobile", score: 83, tags: ["active"] },
  { id: 11, name: "Olim", age: 21, major: "Backend", score: 68, tags: ["researcher"] },
  { id: 12, name: "Nodira", age: 20, major: "Design", score: 87, tags: ["creative"] },
  { id: 13, name: "Shaxzod", age: 25, major: "Mobile", score: 45, tags: ["borderline"] },
  { id: 14, name: "Malika", age: 22, major: "Frontend", score: 94, tags: ["top", "creative"] },
  { id: 15, name: "Rustam", age: 23, major: "Backend", score: 81, tags: ["active"] }
];
// Zanjir 1: Balli 80 dan yuqori bo'lgan Frontendchilarning faqat ismlari
const topFrontenders = (students || [])
  .filter(({ major, score }) => major === "Frontend" && score >= 80)
  .map(({ name }) => name);

// Zanjir 2: "top" tegi bor talabalarning yoshini 1 taga oshirib, yangi obyekt yaratish
const celebratedTopStudents = (students || [])
  .filter(({ tags }) => tags.includes("top"))
  .map(student => ({ ...student, age: student.age + 1 }));

// Zanjir 3: 21 yoshdan katta va balli 70 dan baland talabalarning ID va formatlangan ma'lumoti
const seniorPassList = (students || [])
  .filter(({ age, score }) => age > 21 && score >= 70)
  .map(({ id, name, major }) => ({ id, info: `${name} - ${major}` }));
  // Reduce 1: Mutaxassislik bo'yicha guruhlash (Group By)
const groupByMajor = (students || []).reduce((acc, student) => {
  const { major } = student;
  return {
    ...acc,
    [major]: [...(acc[major] || []), student]
  };
}, {});

// Reduce 2: Ballar statistikasi (Jami ball, Talabalar soni, O'rtacha ball)
const scoreStats = (students || []).reduce((acc, { score }) => {
  const nextCount = acc.count + 1;
  const nextTotal = acc.total + score;
  return {
    count: nextCount,
    total: nextTotal,
    average: Number((nextTotal / nextCount).toFixed(2))
  };
}, { count: 0, total: 0, average: 0 }); // Bo'sh massiv uchun xavfsiz default boshlang'ich qiymat
// find: Balli 60 dan past bo'lgan birinchi "borderline" talabani topish
const criticalStudent = (students || []).find(({ score }) => score < 60) || null;

// some: Bazada "mentor"lik tegi bor talaba bormi? (True/False)
const hasMentors = (students || []).some(({ tags }) => tags.includes("mentor"));

// every: Hamma talabalarning balli 40 dan yuqorimi? (True/False)
const isEveryonePassing = (students || []).every(({ score }) => score >= 40);
const sortedStudents = [...(students || [])].sort((a, b) => {
  // Mezonlar tuple ko'rinishida: [ball farqi, yosh farqi]
  const criteria = [b.score - a.score, a.age - b.age];
  
  // Birinchi nolga teng bo'lmagan farqni topib qaytaradi (Agarda ballar farqi 0 bo'lsa, yoshga o'tadi)
  return criteria.find(value => value !== 0) || 0;
});
console.log("Top Frontendchilar:", topFrontenders);
console.log("Mutaxassislik bo'yicha guruhlar:", Object.keys(groupByMajor)); // Kalitlarni ko'rish
console.log("Ballar Statistikasi:", scoreStats);
console.log("Xavfli zonadagi talaba:", criticalStudent?.name);
console.log("Mentorlar bormi?:", hasMentors);
console.log("Hamma imtihondan o'tdimi?:", isEveryonePassing);
console.log("Eng yuqori balli talaba (Saralangan):", sortedStudents[0].name);

// Bo'sh massiv uzatilganda xavfsiz ishlash testi
const emptyTest = null;
const safeResult = (emptyTest || []).filter(s => s.score > 50);
console.log("Bo'sh massiv uchun xavfsiz qaytgan qiymat:", safeResult); // []
