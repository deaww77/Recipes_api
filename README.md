# flutter_recipes_api
# Flutter Recipes App 🍳

โปรเจคศึกษาการใช้งาน REST API ผ่าน DummyJSON เพื่อดึงข้อมูลสูตรอาหาร และแสดงผลใน Flutter Application

## 📖 คำอธิบายโปรเจค

แอปพลิเคชันนี้เป็นตัวอย่างการเรียน API จาก [DummyJSON](https://dummyjson.com/) เพื่อดึงข้อมูลสูตรอาหาร และนำมาแสดงผลในรูปแบบ List View ภายใน Flutter App

## ✨ ฟีเจอร์

- ✅ ดึงข้อมูลสูตรอาหารจาก DummyJSON API
- ✅ แสดงรายการสูตรอาหารพร้อมรูปภาพ
- ✅ แสดงข้อมูลพื้นฐาน (ชื่อ, ประเภทอาหาร, ระดับความยาก)
- ✅ Loading state และ Error handling
- ✅ Responsive UI design

## 🛠 เทคโนโลยีที่ใช้

- **Flutter SDK** - Framework สำหรับพัฒนา mobile app
- **Dart** - ภาษาโปรแกรมมิ่ง
- **HTTP Package** - สำหรับเรียก API
- **DummyJSON API** - Mock API สำหรับทดสอบ

## 📁 โครงสร้างโค้ด

### 1. Model Class (Recipe)

```dart
class Recipe {
  final int id;
  final String name;
  final List<String> ingredients;
  // ... properties อื่นๆ
  
  factory Recipe.fromJson(Map<String, dynamic> json) {
    return Recipe(
      id: json['id'],
      name: json['name'],
      ingredients: List<String>.from(json['ingredients']),
      // ... mapping ข้อมูลจาก JSON
    );
  }
}
```

**อธิบาย:**
- สร้าง Model class สำหรับจัดเก็บข้อมูลสูตรอาหาร
- `factory Recipe.fromJson()` ใช้สำหรับแปลงข้อมูล JSON เป็น Object
- `List<String>.from()` ใช้แปลง JSON array เป็น Dart List

### 2. Service Class (RecipsService)

```dart
class RecipsService {
  static const String apiUrl = "https://dummyjson.com/recipes";
  
  static Future<List<Recipe>> fetchRecipes() async {
    final response = await http.get(Uri.parse(apiUrl));
    if (response.statusCode == 200) {
      final data = json.decode(response.body);
      final List<dynamic> recipesJson = data['recipes'];
      return recipesJson.map((json) => Recipe.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load recipes');
    }
  }
}
```

**อธิบาย:**
- `static` method ทำให้เรียกใช้ได้โดยไม่ต้องสร้าง instance
- `Future<List<Recipe>>` คือ asynchronous function ที่คืนค่า List ของ Recipe
- `http.get()` ใช้เรียก API แบบ GET request
- `response.statusCode == 200` ตรวจสอบว่า request สำเร็จ
- `json.decode()` แปลง JSON string เป็น Map
- `.map()` แปลงแต่ละ item ใน array เป็น Recipe object

### 3. UI Widget (RecipesScreen)

```dart
class RecipesScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Recipes")),
      body: FutureBuilder<List<Recipe>>(
        future: RecipsService.fetchRecipes(),
        builder: (context, snapshot) {
          // Handle different states
        },
      ),
    );
  }
}
```

**อธิบาย:**
- `FutureBuilder` ใช้จัดการ asynchronous operations
- `snapshot.connectionState` ตรวจสอบสถานะการโหลดข้อมูล
  - `ConnectionState.waiting` - กำลังโหลด
  - `snapshot.hasError` - เกิดข้อผิดพลาด
  - `snapshot.hasData` - มีข้อมูล
- `ListView.builder` สร้าง scrollable list อย่างมีประสิทธิภาพ

## 🚀 วิธีการรัน

1. Clone repository
```bash
git clone <repository-url>
cd flutter-recipes-app
```

2. ติดตั้ง dependencies
```bash
flutter pub get
```

3. รันแอปพลิเคชัน
```bash
flutter run
```

## 📦 Dependencies

เพิ่มใน `pubspec.yaml`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.1.0
```

## 🔧 API Endpoint

**Base URL:** `https://dummyjson.com/recipes`

**Response Format:**
```json
{
  "recipes": [
    {
      "id": 1,
      "name": "Classic Margherita Pizza",
      "ingredients": ["Pizza dough", "Tomato sauce", "Fresh mozzarella cheese"],
      "instructions": ["Preheat the oven to 475°F (245°C)"],
      "prepTimeMinutes": 20,
      "cookTimeMinutes": 15,
      "servings": 4,
      "difficulty": "Easy",
      "cuisine": "Italian",
      "caloriesPerServing": 300,
      "tags": ["Pizza", "Italian"],
      "userId": 166,
      "image": "https://cdn.dummyjson.com/recipe-images/1.webp",
      "rating": 4.6,
      "reviewCount": 98,
      "mealType": ["Dinner"]
    }
  ]
}
```

## 🎯 สิ่งที่เรียนรู้จากโปรเจคนี้

1. **HTTP Requests** - การเรียก REST API ด้วย http package
2. **JSON Parsing** - การแปลง JSON เป็น Dart objects
3. **Future & Async** - การจัดการ asynchronous operations
4. **State Management** - การใช้ FutureBuilder สำหรับจัดการสถานะ
5. **Error Handling** - การจัดการข้อผิดพลาดจาก API
6. **UI Components** - การใช้ ListView, ListTile, และ Image.network

## 🔄 การพัฒนาต่อ

- [ ] เพิ่มหน้ารายละเอียดสูตรอาหาร
- [ ] เพิ่มฟังก์ชั่น Search และ Filter
- [ ] เพิ่ม Local Storage สำหรับ Favorites
- [ ] ปรับปรุง UI/UX design
- [ ] เพิ่ม Unit Tests

---

**สร้างโดย:** chanon roobkhum  
**วันที่:** (date 8/8/2025)  

