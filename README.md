# 🎓 منصة مسار التعليمية - Masar Educational Platform

مشروع تعليمي متقدم تم تطويره باستخدام **Flutter**، يركز على مفاهيم التنقل بين الشاشات (Navigation) وتبادل البيانات المتغيرة بين الواجهات (Data Passing).

---

## 📸 لقطات الشاشة (Project Showcase)




## 📸 لقطات شاشة المشروع (Project Showcase)
<div align="center">
  <img src="https://raw.githubusercontent.com/Roua122/basic-stack-navigation/main/screenshots/nav1.png" width="250" alt="الشاشة الرئيسية">
  <img src="https://raw.githubusercontent.com/Roua122/basic-stack-navigation/main/screenshots/nav2.png" width="250" alt="شاشة التفاصيل">
  <img src="https://raw.githubusercontent.com/Roua122/basic-stack-navigation/main/screenshots/nav3.png" width="250" alt="تنبيه الاشتراك">
</div>


---

## 📄 التقرير التقني (Technical Insights)

### **1. هندسة الشاشة الرئيسية (HomeScreen):**
* **التخطيط الشبكي:** تم الاعتماد على `GridView.count` بتنسيق ثنائي الأعمدة لضمان عرض المسارات بشكل متجاوب.
* **الديناميكية:** إنشاء دالة `_buildCourseCard` مكننا من بناء واجهة مرنة وسهلة التعديل، مع تقليل تكرار الكود.

### **2. آلية التنقل وتمرير البيانات (Forward Passing):**
* تم استخدام `Navigator.push` لنقل المستخدم من الواجهة العامة إلى واجهة التفاصيل.
* **تمرير البيانات:** قمت بتمرير **6 متغيرات** حيوية عبر الـ Constructor (الاسم، المدرب، المدة، السعر، الأيقونة، واللون).

### **3. إدارة المكدس وإرجاع النتائج (Stack & Pop):**
* **إغلاق الواجهة:** تم استخدام `Navigator.pop` عند تأكيد الاشتراك.
* **البيانات الراجعة:** برمجت دالة الـ Pop لتعيد رسالة نجاح مخصصة تحتوي على اسم الكورس المختار.

---

## 💻 الكود البرمجي الكامل (Full Source Code)

```dart
import 'package:flutter/material.dart';

void main() => runApp(
  const MaterialApp(debugShowCheckedModeBanner: false, home: HomeScreen()),
);

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: const Color(0xFF0F172A),
      appBar: AppBar(
        title: const Text('منصة مسار التعليمية', style: TextStyle(fontWeight: FontWeight.bold, color: Colors.white)),
        backgroundColor: const Color(0xFF0F172A),
        centerTitle: true,
      ),
      body: GridView.count(
        crossAxisCount: 2,
        padding: const EdgeInsets.all(20),
        crossAxisSpacing: 15,
        mainAxisSpacing: 15,
        children: [
          _buildCourseCard(context, 'تطوير Flutter', Icons.phone_android, Colors.blueAccent, "م. رؤى محمد", "30 ساعة", "مجاني"),
          _buildCourseCard(context, 'الأمن السيبراني', Icons.security, Colors.redAccent, "م. أحمد سالم", "45 ساعة", "مجاني"),
        ],
      ),
    );
  }

  Widget _buildCourseCard(BuildContext context, String title, IconData icon, Color color, String instructor, String duration, String price) {
    return InkWell(
      onTap: () async {
        final result = await Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => DetailScreen(
              courseName: title,
              icon: icon,
              iconColor: color,
              instructor: instructor,
              duration: duration,
              price: price,
            ),
          ),
        );
        if (result != null && context.mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text(result.toString(), textAlign: TextAlign.right),
              backgroundColor: color,
            ),
          );
        }
      },
      child: Container(
        decoration: BoxDecoration(
          color: const Color(0xFF1E293B),
          borderRadius: BorderRadius.circular(15),
        ),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(icon, size: 40, color: color),
            const SizedBox(height: 10),
            Text(title, style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
          ],
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  final String courseName, instructor, duration, price;
  final IconData icon;
  final Color iconColor;

  const DetailScreen({
    super.key,
    required this.courseName, required this.icon, required this.iconColor,
    required this.instructor, required this.duration, required this.price,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: const Color(0xFF0F172A),
      appBar: AppBar(backgroundColor: const Color(0xFF0F172A), iconTheme: const IconThemeData(color: Colors.white)),
      body: Center(
        child: Container(
          margin: const EdgeInsets.all(20),
          padding: const EdgeInsets.all(25),
          decoration: BoxDecoration(color: const Color(0xFF1E293B), borderRadius: BorderRadius.circular(20)),
          child: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.end,
            children: [
              Center(child: Icon(icon, size: 70, color: iconColor)),
              const SizedBox(height: 15),
              Center(child: Text(courseName, style: const TextStyle(fontSize: 24, color: Colors.white, fontWeight: FontWeight.bold))),
              const Divider(color: Colors.white24, height: 30),
              _buildDetailRow(Icons.person, "المقدم:", instructor),
              _buildDetailRow(Icons.timer, "المدة:", duration),
              _buildDetailRow(Icons.payments, "السعر:", price),
              const SizedBox(height: 30),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton.icon(
                  onPressed: () => Navigator.pop(context, '✅ تم الاشتراك في $courseName'),
                  icon: const Icon(Icons.check_circle, color: Colors.white),
                  label: const Text('تأكيد الاشتراك', style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
                  style: ElevatedButton.styleFrom(backgroundColor: iconColor, padding: const EdgeInsets.symmetric(vertical: 15)),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildDetailRow(IconData icon, String label, String value) {
    return Directionality(
      textDirection: TextDirection.rtl,
      child: Padding(
        padding: const EdgeInsets.symmetric(vertical: 8),
        child: Row(
          children: [
            Icon(icon, size: 22, color: iconColor.withOpacity(0.8)),
            const SizedBox(width: 10),
            Text(label, style: const TextStyle(color: Colors.blueGrey, fontSize: 16)),
            const SizedBox(width: 8),
            Text(value, style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
          ],
        ),
      ),
    );
  }
}
