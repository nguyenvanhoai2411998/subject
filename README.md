//  밑에는 키바나 링크입니다:

//  http://20.249.72.133:5601/

//  대시보드안에는 충전소의 위치, 충전기 상태, 지역별로 충전소, 등이 있습니다.

// azure id: minjoo
//       pw: NguyenHo@i1998



//xml file 받기 위해 자바 코드
//한국환경공단_전기자동차 충전소 정보를 파싱하기
package ElectricCar_Charger;

import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;

import org.w3c.dom.Document;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import java.io.BufferedReader;
import java.io.FileOutputStream;
import java.io.IOException;

public class GetChargerInfo {
	
	private static final String USER_AGENT = "Mozilla/5.0";
	
    public static void main(String[] args) throws IOException {
    	
    		StringBuilder urlBuilder = new StringBuilder("http://apis.data.go.kr/B552584/EvCharger/getChargerInfo"); /*URL*/  		
            urlBuilder.append("?" + URLEncoder.encode("serviceKey","UTF-8") + "=FXeDauu0lqS3%2FoBtaJhtpwGWiH6Q6quB%2FgQuQECFP7Hg%2BOR6gKsjCCulHSd%2FLY9CP7Mdw5BIlcbu7fP2X1sanQ%3D%3D"); /*Service Key*/
            urlBuilder.append("&" + URLEncoder.encode("pageNo","UTF-8") + "=" + URLEncoder.encode("1", "UTF-8")); /*페이지 번호*/
            urlBuilder.append("&" + URLEncoder.encode("numOfRows","UTF-8") + "=" + URLEncoder.encode("10", "UTF-8")); /*한 페이지 결과 수 (최소 10, 최대 9999)*/
            urlBuilder.append("&" + URLEncoder.encode("period","UTF-8") + "=" + URLEncoder.encode("5", "UTF-8")); /*상태갱신 조회 범위(분) (기본값 5, 최소 1, 최대 10)*/
            urlBuilder.append("&" + URLEncoder.encode("zcode","UTF-8") + "=" + URLEncoder.encode("11", "UTF-8")); /*시도 코드 (행정구역코드 앞 2자리)*/
            
            
            
            URL url = new URL(urlBuilder.toString());
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");
            conn.setRequestProperty("User-Agent", USER_AGENT);
            System.out.println("Response code: " + conn.getResponseCode());
            BufferedReader rd;
            if(conn.getResponseCode() >= 200 && conn.getResponseCode() <= 300) {
                rd = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            } else {
                rd = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
            }
            StringBuilder sb = new StringBuilder();
            String line;
            while ((line = rd.readLine()) != null) {
            	//sb에 데이터 쓰기
                sb.append(line);
            }
            rd.close();
            try {
    			DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
    			DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();

    			FileOutputStream output = new FileOutputStream("./GetChargerInfo");
    			output.write(sb.toString().getBytes());
    			output.close();
    			Document doc = dBuilder.parse("./GetChargerInfo");
    			doc.getDocumentElement().normalize();
    			
    			
    		} catch (Exception e) {
    			e.printStackTrace();
    		}
            //print result
            conn.disconnect();
            System.out.println(sb.toString());
    	
    }
}

