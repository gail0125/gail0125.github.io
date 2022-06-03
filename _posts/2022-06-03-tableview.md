---
layout: post
title:  "12장 tableview"
---
#12장

TableviewController.swift

import UIKit

var items = ["책 구매", "철수와 약속", "스터디 준비하기"]
var itemsImageFile = ["cart.png", "clock.png", "pencil.png"]

class TableViewController: UITableViewController {

    @IBOutlet var tvListView: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Uncomment the following line to preserve selection between presentations
        // self.clearsSelectionOnViewWillAppear = false

        // Uncomment the following line to display an Edit button in the navigation bar for this view controller.
        self.navigationItem.leftBarButtonItem = self.editButtonItem
    }
    
    
    override func viewWillAppear(_ animated: Bool) {
            tvListView.reloadData()
    }
    

    // MARK: - Table view data source

    override func numberOfSections(in tableView: UITableView) -> Int {
        // #warning Incomplete implementation, return the number of sections
        return 1
    }

    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // #warning Incomplete implementation, return the number of rows
        return items.count
    }

    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "myCell", for: indexPath)

        cell.textLabel?.text = items[(indexPath as NSIndexPath).row]
        cell.imageView?.image = UIImage(named: itemsImageFile[(indexPath as NSIndexPath).row])

        return cell
    }
    

    /*
    // Override to support conditional editing of the table view.
    override func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {
        // Return false if you do not want the specified item to be editable.
        return true
    }
    */

    
    // Override to support editing the table view.
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
            // Delete the row from the data source
            items.remove(at: (indexPath as NSIndexPath).row)
            itemsImageFile.remove(at: (indexPath as NSIndexPath).row)
            tableView.deleteRows(at: [indexPath], with: .fade)
        } else if editingStyle == .insert {
            // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
        }
    }
    
    override func tableView(_ tableView: UITableView, titleForDeleteConfirmationButtonForRowAt indexPath: IndexPath) -> String? {
        return "삭제"
    }
    

    
    // Override to support rearranging the table view.
    override func tableView(_ tableView: UITableView, moveRowAt fromIndexPath: IndexPath, to: IndexPath) {
        let itemToMove = items[(fromIndexPath as NSIndexPath).row]
        let itemImageToMove = itemsImageFile[(fromIndexPath as NSIndexPath).row]
        items.remove(at: (fromIndexPath as NSIndexPath).row)
        itemsImageFile.remove(at: (fromIndexPath as NSIndexPath).row)
        items.insert(itemToMove, at: (to as NSIndexPath).row)
        itemsImageFile.insert(itemImageToMove, at: (to as NSIndexPath).row)
    }
    

    /*
    // Override to support conditional rearranging of the table view.
    override func tableView(_ tableView: UITableView, canMoveRowAt indexPath: IndexPath) -> Bool {
        // Return false if you do not want the item to be re-orderable.
        return true
    }
    */

    
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
        if segue.identifier == "sgDetail" {
            let cell = sender as! UITableViewCell
            let indexPath = self.tvListView.indexPath(for: cell)
            let detailView = segue.destination as! DetailViewController
            detailView.reciveItem(items[((indexPath! as NSIndexPath).row)])
        }
    }
    

}


AddViewController.swift


import UIKit

class AddViewController: UIViewController {

    @IBOutlet var tfAddItem: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
    }
    
    @IBAction func btnAddItem(_ sender: UIButton) {
        items.append(tfAddItem.text!)
        itemsImageFile.append("clock.png")
        tfAddItem.text=""
        _ = navigationController?.popViewController(animated: true)
    }
    
    /*
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
    }
    */

}


DetailViewController.swift


import UIKit

class DetailViewController: UIViewController {
    
    var receiveItem = ""

    @IBOutlet var lblItem: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
        lblItem.text = receiveItem
    }
    
    
    func reciveItem(_ item: String)
    {
        receiveItem = item
    }
    

    /*
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
    }
    */

}


위 코드중 TableViewController에는 테이블 목록을 보여주는 코드와 그 목록을 삭제하는 코드와 목록 순서를 바꾸는 코드가 포함되어 있습니다.
AddViewController에는 새 목록을 추가하는 코드,DetailViewController에는 목록의 세부내용을 보여주는 코드가 포함되어 있습니다.
TableViewController중 목록을 보여주는 코드에는 이미지의 변수를 설정한 다음 선언한 변수를 셀에 적용하는 함수를 활성화시켜 설정한 변수가 셀에 적용되어 있습니다. 목록 삭제 코드에는 주석문으로 되어있던 함수를 활성화시켜서 선택한셀을 삭제할수있게 하였습니다. 이러면 목록을 밀어서 삭제하는 것이 가능합니다. 이방법 뿐만아니라 바 버튼으로 목록을 삭제하는 방법도 있는데 //self.navigationItem.leftBarButtonItem = self.editButtonItem 에서 앞에 있는 //(주석)을 지워 바버튼을 활성화 시켰습니다. 
목록 순서를 바꾸는 코드에서는 /* tableView(_tableView:UITableView,moveRowAt fromIndexPath: IndexPath, to: IndexPath)*/ 함수의 주석을 제거하고 이함수에 목록을 다른곳으로 이동하는 소스를 추가해서 목록 순서변경 코드를 만들었습니다.
AddViewController의 목록을 추가하는 코드는 버튼 함수를 수정해서
@IBAction func btnAddItem(_ sender: UIButton) {
        items.append(tfAddItem.text!)
        itemsImageFile.append("clock.png")
        tfAddItem.text=""
        _ = navigationController?.popViewController(animated: true)
    }
이런식으로 수정했습니다. 이소스의 의미는 items에 텍스트 필드의 값을 추가하고 텍스트 필드의 내용을 지운다음 itemsImageFile에는 무조건 clock.png 파일을 추가한다음 뷰 컨트롤러로 돌아간다. 라는 의미입니다.

그 다음 목록추가 동작 코드는 다음과 같습니다.
override func viewWillAppear(_ animated: Bool) {
            tvListView.reloadData()
    }
이것으로 Add뷰 동작 코드는 완성됬습니다.
그 다음은 마지막으로 DetailViewController의 목록의 세부 내용을 보여주는 코드class DetailViewController: UIViewController {
    
    var receiveItem = ""

    @IBOutlet var lblItem: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
        lblItem.text = receiveItem
    }
    
    
    func reciveItem(_ item: String)
    {
        receiveItem = item
    }
위 소스의 의미는 메인 뷰에서 받을 텍스트를 위해 변수 receiveitem을 선언,  뷰가 노출될때마다 레이블의 텍스로 표시, 메인뷰에서 변수를 받기위한 함수를 추가한다 를 의미합니다.
다음은 테이블뷰에서 세그웨이를 이용하여 뷰를 이동하는 함수를 활성화시켰습니다.
코드는 아래와 같습니다.Override to support conditional rearranging of the table view.
    override func tableView(_ tableView: UITableView, canMoveRowAt indexPath: IndexPath) -> Bool {
        // Return false if you do not want the item to be re-orderable.
        return true
    }
    */

    
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
        if segue.identifier == "sgDetail" {
            let cell = sender as! UITableViewCell
            let indexPath = self.tvListView.indexPath(for: cell)
            let detailView = segue.destination as! DetailViewController
            detailView.reciveItem(items[((indexPath! as NSIndexPath).row)])
        }
    }
    
이상으로 테이블 뷰와 애드뷰 디테일뷰를 이용한 목록만들기였습니다.

