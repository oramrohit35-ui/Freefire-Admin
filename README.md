# Freefire-Adminimport 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class RoomDetailsScreen extends StatelessWidget {
  final String matchId;

  RoomDetailsScreen({required this.matchId});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Room Details")),
      body: StreamBuilder(
        // Firestore se real-time data lena
        stream: FirebaseFirestore.instance.collection('tournaments').doc(matchId).snapshots(),
        builder: (context, AsyncSnapshot<DocumentSnapshot> snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());

          var matchData = snapshot.data!;
          String roomId = matchData['roomId'];
          String roomPass = matchData['roomPassword'];

          return Padding(
            padding: EdgeInsets.all(20),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text("Match Room Details", style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
                SizedBox(height: 30),
                
                // Room ID Box
                Container(
                  padding: EdgeInsets.all(15),
                  color: Colors.grey[800],
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Text("Room ID: $roomId", style: TextStyle(fontSize: 18)),
                      Icon(Icons.copy, color: Colors.white), // Copy karne ka button yahan lagayein
                    ],
                  ),
                ),
                SizedBox(height: 15),

                // Password Box
                Container(
                  padding: EdgeInsets.all(15),
                  color: Colors.grey[800],
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Text("Password: $roomPass", style: TextStyle(fontSize: 18)),
                      Icon(Icons.copy, color: Colors.white),
                    ],
                  ),
                ),
                SizedBox(height: 30),
                Text(
                  "Note: Please don't share Room ID and Password with anyone else.",
                  textAlign: TextAlign.center,
                  style: TextStyle(color: Colors.redAccent),
                )
              ],
            ),
          );
        },
      ),
    );
  }
}