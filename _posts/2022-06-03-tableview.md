---
layout: post
title:  "12장 tableview"
---
#12장

![12장 1번](../images/2022-06-03-tableview/12장 1번.png)

![12장 2번](../images/2022-06-03-tableview/12장 2번.png)



![12장 3번](../images/2022-06-03-tableview/12장 3번.png)



위 코드중 TableViewController에는 테이블 목록을 보여주는 코드와 그 목록을 삭제하는 코드와 목록 순서를 바꾸는 코드가 포함되어 있습니다.
AddViewController에는 새 목록을 추가하는 코드,DetailViewController에는 목록의 세부내용을 보여주는 코드가 포함되어 있습니다.
TableViewController중 목록을 보여주는 코드에는 이미지의 변수를 설정한 다음 선언한 변수를 셀에 적용하는 함수를 활성화시켜 설정한 변수가 셀에 적용되어 있습니다. 
목록 삭제 코드에는 주석문으로 되어있던 함수를 활성화시켜서 선택한셀을 삭제할수있게 하였습니다. 
이러면 목록을 밀어서 삭제하는 것이 가능합니다. 
이방법 뿐만아니라 바 버튼으로 목록을 삭제하는 방법도 있는데 //self.navigationItem.leftBarButtonItem = self.editButtonItem 에서 앞에 있는 //(주석)을 지워 바버튼을 활성화 시켰습니다. 
목록 순서를 바꾸는 코드에서는 /* tableView(_tableView:UITableView,moveRowAt fromIndexPath: IndexPath, to: IndexPath)*/ 함수의 주석을 제거하고 이함수에 목록을 다른곳으로 이동하는 소스를 추가해서 목록 순서변경 코드를 만들었습니다.
AddViewController의 목록을 추가하는 코드는 버튼 함수를 수정해서

@IBAction func btnAddItem(_ sender: UIButton) {
        items.append(tfAddItem.text!)
        itemsImageFile.append("clock.png")
        tfAddItem.text=""
        _ = navigationController?.popViewController(animated: true)
    }
    
이런식으로 수정했습니다. 
이소스의 의미는 items에 텍스트 필드의 값을 추가하고 텍스트 필드의 내용을 지운다음 itemsImageFile에는 무조건 clock.png 파일을 추가한다음 뷰 컨트롤러로 돌아간다. 라는 의미입니다.

그 다음 목록추가 동작 코드는 다음과 같습니다.

override func viewWillAppear(_ animated: Bool) {
            tvListView.reloadData()
    }
    
이것으로 Add뷰 동작 코드는 완성됬습니다.
그 다음은 마지막으로 DetailViewController의 목록의 세부 내용을 보여주는 코드

class DetailViewController: UIViewController {
    
    var receiveItem = ""
    
    @IBOutlet var lblItem: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    
        // Do any additional setup after loading the view.
        lblItem.text = receiveItem
    }


​    
​    func reciveItem(_ item: String)
​    {
​        receiveItem = item
​    }

위 소스의 의미는 메인 뷰에서 받을 텍스트를 위해 변수 receiveitem을 선언,  뷰가 노출될때마다 레이블의 텍스로 표시, 메인뷰에서 변수를 받기위한 함수를 추가한다 를 의미합니다.
다음은 테이블뷰에서 세그웨이를 이용하여 뷰를 이동하는 함수를 활성화시켰습니다.
코드는 아래와 같습니다.

Override to support conditional rearranging of the table view.
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



앱의 환경은 이렇습니다.



![스크린샷 2022-06-16 오후 3.41.39](../images/2022-06-03-tableview/스크린샷 2022-06-16 오후 3.41.39.png)

아래는 앱의 동작사진입니다. 



![12(1)](../images/2022-06-03-tableview/12(1).png)

이게 앱을 실행하면 처음으로 나오는 화면입니다. 해당 목록을 클릭하면

![12장 디테일](../images/2022-06-03-tableview/12장 디테일-16553734222628.png)

이렇게 세부내용을 볼수있습니다. main view 버튼을 눌러 초기화면으로 돌아온뒤

![12(1)](../images/2022-06-03-tableview/12(1)-165537349597610.png)

우측 +버튼을 눌러줍니다.

![12(2)](../images/2022-06-03-tableview/12(2).png)

그러면 이렇게 Add View창에 들어가게 되고 

![12(3)](../images/2022-06-03-tableview/12(3).png)

중앙에 있는 텍스트 필드에 텍스트를 입력하고 add버튼을 누르면

![12(4)](../images/2022-06-03-tableview/12(4).png)

이렇게 목록이 늘어납니다. 그다음 좌측의 edit버튼을 누르면

![12(5)](../images/2022-06-03-tableview/12(5).png)

이런 화면이 되며 우측 라인에 막대기3개 아이콘을 위로 잡고 올리면

![12(6)](../images/2022-06-03-tableview/12(6).png)

이렇게 순서도 바꿀수 있습니다. 그 다음 좌측 -를 누르면

![12(7)](../images/2022-06-03-tableview/12(7).png)

이렇게 선택 라인의 삭제 창이 뜨면서 삭제 창을 누르면 삭제가 완료된다.

![12(8)](../images/2022-06-03-tableview/12(8).png)



여기까지가 테이블뷰 컨트롤러를 이용한 목록 앱  동작이다
