/* Flutter Social Platform Starter

Features:

Firebase Authentication (Login/Signup)

Basic Feed with Photo Upload (placeholder)

Stories placeholder

Reels placeholder

Chat placeholder


👉 Before running:

1. Create a Firebase project (https://firebase.google.com/)


2. Enable Authentication (Email/Google)


3. Enable Firestore Database + Storage


4. Add google-services.json (Android) / GoogleService-Info.plist (iOS)


5. Add required dependencies in pubspec.yaml:

dependencies: flutter: sdk: flutter firebase_core: ^2.15.1 firebase_auth: ^4.7.2 cloud_firestore: ^4.9.1 firebase_storage: ^11.2.6 image_picker: ^1.0.4



*/

import 'package:flutter/material.dart'; import 'package:firebase_core/firebase_core.dart'; import 'package:firebase_auth/firebase_auth.dart';

Future<void> main() async { WidgetsFlutterBinding.ensureInitialized(); await Firebase.initializeApp(); runApp(SocialApp()); }

class SocialApp extends StatelessWidget { @override Widget build(BuildContext context) { return MaterialApp( debugShowCheckedModeBanner: false, title: 'Social Platform', theme: ThemeData.dark(), home: AuthGate(), ); } }

// 🔹 Auth Gate (Login / Home) class AuthGate extends StatelessWidget { @override Widget build(BuildContext context) { return StreamBuilder<User?>( stream: FirebaseAuth.instance.authStateChanges(), builder: (context, snapshot) { if (snapshot.connectionState == ConnectionState.waiting) { return Scaffold(body: Center(child: CircularProgressIndicator())); } if (snapshot.hasData) { return HomeScreen(); } return LoginScreen(); }, ); } }

// 🔹 Login Screen class LoginScreen extends StatefulWidget { @override _LoginScreenState createState() => _LoginScreenState(); }

class _LoginScreenState extends State<LoginScreen> { final emailController = TextEditingController(); final passwordController = TextEditingController();

void login() async { try { await FirebaseAuth.instance.signInWithEmailAndPassword( email: emailController.text.trim(), password: passwordController.text.trim(), ); } catch (e) { ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(e.toString()))); } }

void signup() async { try { await FirebaseAuth.instance.createUserWithEmailAndPassword( email: emailController.text.trim(), password: passwordController.text.trim(), ); } catch (e) { ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(e.toString()))); } }

@override Widget build(BuildContext context) { return Scaffold( body: Padding( padding: const EdgeInsets.all(20), child: Column( mainAxisAlignment: MainAxisAlignment.center, children: [ TextField(controller: emailController, decoration: InputDecoration(labelText: "Email")), TextField(controller: passwordController, obscureText: true, decoration: InputDecoration(labelText: "Password")), SizedBox(height: 20), ElevatedButton(onPressed: login, child: Text("Login")), TextButton(onPressed: signup, child: Text("Sign Up")), ], ), ), ); } }

// 🔹 Home Screen with Bottom Navigation class HomeScreen extends StatefulWidget { @override _HomeScreenState createState() => _HomeScreenState(); }

class _HomeScreenState extends State<HomeScreen> { int _selectedIndex = 0;

final List<Widget> _pages = [ FeedPage(), ReelsPage(), StoryPage(), ChatPage(), ProfilePage(), ];

@override Widget build(BuildContext context) { return Scaffold( body: _pages[_selectedIndex], bottomNavigationBar: BottomNavigationBar( currentIndex: _selectedIndex, onTap: (index) => setState(() => _selectedIndex = index), items: const [ BottomNavigationBarItem(icon: Icon(Icons.home), label: "Feed"), BottomNavigationBarItem(icon: Icon(Icons.video_library), label: "Reels"), BottomNavigationBarItem(icon: Icon(Icons.add_circle), label: "Story"), BottomNavigationBarItem(icon: Icon(Icons.chat), label: "Chat"), BottomNavigationBarItem(icon: Icon(Icons.person), label: "Profile"), ], ), ); } }

// 🔹 Placeholder Pages class FeedPage extends StatelessWidget {

