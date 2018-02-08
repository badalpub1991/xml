Swift Data :-

1) For open Emojis in text press ctrl + command + Space
2) For Select Color programatically ==> Color Literal 

==> // Turnery Operator in Swift:-
SYNTAX :- value = condition ? valueIfTrue : valueIfFalse
EX :- cell.acessoryType = item.done==True ? .checkmark : .none
DETAIL CONDITION :-
     if item.done == true {
         cell.accessoryType = .checkmark
     } else {
          cell.accessoryType = .none
     }
     
==> // Selected Not Selected in Single Line :-

 itamArry[indexpath.row].done = !itemArry[indexpath.row].done
DETAIL CONDITION :- 
    if itemArry[indexpath.row].done == false {
        itemArry[indexpath.row].done = true
    } else {
        itemArry[indexpath.row].done = false
    }

==> //Structure 

   1) MVC 2) Service 3) Utilities(Constant Files) 4) XIB's (If Available)
   
==> SingleTone Example in array

1)     import Foundation
       import Alamofire
       import SwiftyJSON
       class AuthService {
       static let instance = AuthService() // Required in SingleTone files
       
2) Constant files Complition Handler 

       typealias ComplitionHandler = (_ Success :Bool) -> ()
       usage:- //Register User with Alamofire
       
        func registerUser (email : String, password: String, completion: @escaping ComplitionHandler){
        let lowerCaseEmail = email.lowercased()
      
        
        let body : [String : Any] = [
            "email" : lowerCaseEmail,
            "password": password
        ]
        
        Alamofire.request(URL_REGISTER, method: .post, parameters: body, encoding: JSONEncoding.default, headers: HEADER).responseString { (response) in
            if response.result.error == nil {
                completion(true)
            } else {
                completion(false)
                debugPrint(response.result.error as Any)
            }
        }
        
       }
       
---

            //Login User with Alamofire
    func loginUser (email : String, password: String, completion: @escaping ComplitionHandler) {
        let lowerCaseEmail = email.lowercased()
        
        
        let body : [String : Any] = [
            "email" : lowerCaseEmail,
            "password": password
        ]
        
        Alamofire.request(URL_LOGIN, method: .post, parameters: body, encoding: JSONEncoding.default, headers: HEADER).responseString { (response) in
            if response.result.error == nil {
                
              /*  if let json = response.result.value as? Dictionary<String,Any> {
                    if let email = json ["user"] as? String {
                        self.userEmail = email
                    }
                    if let token = json ["token"] as? String {
                        self.authToken = token
                    }
                }*/
                
                //Using Swifty JSON
                guard let data = response.data else {return}
                
                do {
                    let json = try JSON(data: data)
                    self.userEmail = json["user"].stringValue
                    self.authToken = json["token"].stringValue
                    print(json)
                } catch {
                    print(error)
                    // or display a dialog
                }
                self.isLoggedIn = true
               
                completion(true)
            } else {
                completion(false)
                debugPrint(response.result.error as Any)
            }
        }
    }
    
3) Creating Model Swift

           import Foundation
           struct Channel {
           public private(set) var channelTitle : String!
           public private(set) var channelDescription : String!
           public private(set) var id : String!
          }
         // var channels = [Channel]() //Array of Channel Object

       //Swift 4 way
       /*struct Channel: Decodable {    //All key in Same Order and As Same name for Encode and Decode Json as Swift4
       public private(set) var _id : String!
       public private(set) var name : String!
       public private(set) var description : String!
       public private(set) var __v : Int?
       }*/ 
             // MARK:- Find All Channel
       func findAllChannel(complition: @escaping ComplitionHandler) {
        Alamofire.request(URL_GET_CHANNELS, method: .get, parameters: nil, encoding: JSONEncoding.default, headers: BEARER_HEADER).responseJSON { (response) in
            if response.result.error == nil {
                guard let data = response.data else {return}
                
                //Swift 4 way in one line
              /*  do {
                    self.channels = try JSONDecoder().decode([Channel].self, from: data)
                    print(self.channels)
                } catch let error {
                    debugPrint(error as Any)
                }*/
                
                //Default Way (Preferable Way)
                do {
                    if let json = try JSON(data:data).array {
                        for item in json {
                            let name = item["name"].stringValue
                            let channelDescription = item["description"].stringValue
                            let id = item["_id"].stringValue
                            let channel = Channel (channelTitle: name, channelDescription: channelDescription, id: id)
                            self.channels.append(channel)
                        }
                       
                    }
                    
                } catch {
                    print(error)
                }
                
                NotificationCenter.default.post(name: NOTIF_CHANNELS_LOADED, object: nil)
                complition(true)
                
            } else {
                complition(false)
                print(response.result.error as Any)
            }
        }
       }
       
4) Creating Array in Swift
            
             var yourArray = [String]()
             
5) Creating Dictionary in Swift
   
             https://www.agnosticdev.com/content/how-work-dictionaries-swift-4

