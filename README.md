# future_builder1_API
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(MaterialApp(
    home: Home(),
  ));
}

class Home extends StatefulWidget {
  const Home({Key? key}) : super(key: key);

  @override
  State<Home> createState() => _HomeState();
}

class _HomeState extends State<Home> {
  var url = "https://api.publicapis.org/entries";
  getdata() async {
    var responce = await http.get(Uri.parse(url));
    var data = json.decode(responce.body);
    return data;
  }

  //var hasData = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          elevation: 1.2,
          backgroundColor: Colors.black,
          title: Text(
            'Portfolio App',
            style: TextStyle(color: Colors.amber),
          ),
        ),
        body: FutureBuilder(
            future: getdata(),
            builder: (context, dynamic snapshot) {
              if (snapshot.hasData) {
                return ListView.builder(
                    itemCount: snapshot.data['entries'].lenght,
                    itemBuilder: ((context, index) {
                      return Card(
                        child: ListTile(
                          title: Text("${snapshot.data['entries'][index]['API']}"),
                        ),
                      );
                    }));
              } else {
                return Center(
                  child: CircularProgressIndicator(),
                );
              }
            }));
  }
}
