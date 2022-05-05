---
layout: project
title: 'Shine Bright'
caption: Google Play 출시 힐링 게임
description: >
  2019년 1월 17일에 Google Play에 출시한 힐링 게임입니다.
date: 16 jan 2019
image: 
  path: /assets/img/projects/shineBright/ShineBright01.gif
links:
  - title: Google Play Link
    url: https://play.google.com/store/apps/details?id=com.TeamSalmon.ShineBright
accent_color: '#4fb1ba'
accent_image:
  background: '#193747'
theme_color: '#193747'
sitemap: false
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

<img src="/assets/img/projects/shineBright/ShineBright02.gif" width="45%"> 
<img src="/assets/img/projects/shineBright/ShineBright03.gif" width="45%">

# 📽️ 소개 영상

샤인 브라이트 트레일러
{% include youtube.html id="BFq6MOWphZw" %}

# 🏆 수상 이력

<img src="/assets/img/projects/shineBright/ShineBright_prize1.png" width="45%"> 
<img src="/assets/img/projects/shineBright/ShineBright_prize2.png" width="45%">

- 성남 커넥트 게임캠프
: 성남산업진흥원 원장상

# 🚀프로젝트 소개

**모바일 캐주얼 게임**

'비'를 테마로 진행되는 감성 위주의 캐주얼 모바일 게임.

창문 밖 비 오는 도시의 야경 속에서 빛을 따라 이동합니다.

<img src="/assets/img/projects/shineBright/ShineBright_app1.png" width="45%"> 
<img src="/assets/img/projects/shineBright/ShineBright_app2.png" width="45%">

구글 플레이에 샤인 브라이트라는 이름으로 2019년 1월에 출시하였습니다.
👉 *[Google Play: 샤인브라이트 링크](https://play.google.com/store/apps/details?id=com.TeamSalmon.ShineBright)*

한 달 뒤, 1000 다운로드 가량을 달성하였습니다.

현재는 5000 다운로드 이상을 기록중입니다.

# 개요

- Unity 3d, C# 사용
- 기간: 2018.10.26 ~ 2019.01.16 (3개월)
- 출시일: 2019.1.17
- 인원: **프로그래머 2인**, 아트 1인

# 참여 내용

## 🔧 Game System

전체적인 게임의 흐름을 담당하는 시스템입니다.
{:.message}

- Enum Switch 상태머신을 이용하여 게임 흐름을 제어합니다.
- 인트로, 본게임, 일시정지, 게임오버의 게임 상태가 있습니다.
- 각 단계별로 매니저들을 참조해 UI와 오디오, 스크립트 제어를 합니다.
- 각 매니저들은 싱글톤으로 처리하였습니다.

## 🕯️ Light System

 빛이 생성되는 궤적을 담당하는 시스템입니다.
{:.message}

- 생성 시간: 빛의 크기에 따라서 다음 빛의 생성시간을 계산하여 생성합니다.
- 생성 위치: 이전에 생성된 위치에서 반지름을 계산하여 연결되도록 생성합니다.
- 크기, 색을 랜덤으로 설정하고, 진폭을 결정합니다
- 게임재화를 빛 근처에 위치하도록 확률적으로 위치를 조정합니다.

## 🎵 Audio System

 사용자 기기에 저장되어있는 음악을 읽어오고 관리합니다.
{:.message}

- 오디오 소스를 따로 두어 빗소리와 음악소리를 따로 조절 가능하도록 구현하였습니다.
- 사용자의 음악을 File을 이용하여 안드로이드에 있는 *.mp3 파일을 스캔 및 플레이 합니다
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
    ```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using System.IO;
    using UnityEngine.UI;
    
    public class FileReader : MonoBehaviour
    {
    
        #region 디렉토리/확장자 관련
        // 루트가 되는 디렉토리를 stirng값으로 저장해놓기
        string[] androidRootlDirs = new string[]
        {
            "/storage",
            "/sdcard",
            "/mnt/sdcard",
            "/mnt/extSdCard"
        };
        string[] windowRootlDirs = new string[]
        {
            @"E:\"
        };
    
        // 찾아올 파일 확장자들
        string[] fildTypes = new string[]
        {
            "*.mp3",
            "*.wav"
        };
        // 세부 디렉토리
        // 파일정보
        DirectoryInfo[] rootFolder;
        FileInfo[] subFile;
        #endregion
    
        // 오디오 소스
        AudioSource audios;
        // 노래목록
        List<string> rootPaths = new List<string>();
        List<string> clipPaths = new List<string>();
        [HideInInspector]
        public List<AudioClip> clips = new List<AudioClip>();
    
        // 저장용
        string tempString = "";
        string[] clipsFullPaths;
    
        public Transform gridImage;
        // 버튼 풀
        public GameObject customClipButtonFactory;
        public GameObject defaultClipButtonFactory;
        int clipListCount = 10;
        GameObject[] customClipButtonPool;
        List<GameObject> customPlusClips = new List<GameObject>();
        GameObject[] defaultClipButtonPool;
    
        // 중복인지 검사
        bool isTwice;
    
        // UI
        public Text searchMusicText;
    
        // 싱글톤
        public static FileReader Instance;
        private void Awake()
        {
            if (Instance == null)
            {
                Instance = this;
            }
        }
    
        // Use this for initialization
        void Start()
        {
            audios = GameObject.Find("AudioManager").GetComponent<AudioSource>();
    
            // 클립버튼 풀
            customClipButtonPool = new GameObject[clipListCount];
            for (int i = 0; i < clipListCount; i++)
            {
                GameObject customClipButton = Instantiate(customClipButtonFactory, gridImage);
                customClipButtonPool[i] = customClipButton;
                customClipButtonPool[i].SetActive(false);
            }
            defaultClipButtonPool = new GameObject[AudioManager.Instance.bgmClips.Length];
            for (int i = 0; i < defaultClipButtonPool.Length; i++)
            {
                GameObject defaultClipButton = Instantiate(defaultClipButtonFactory, gridImage);
                defaultClipButtonPool[i] = defaultClipButton;
                defaultClipButtonPool[i].SetActive(false);
            }
    
            // 경로 불러오기
            if (PlayerPrefs.HasKey("ClipsPaths"))
            {
                clipsFullPaths = PlayerPrefs.GetString("ClipsPaths").Split(',');
    
                // 패스가 비지 않았다면
                if (clipsFullPaths[0] != "")
                {
                    for (int i = 0; i < clipsFullPaths.Length; i++)
                    {
                        clipPaths.Add(clipsFullPaths[i]);
                    }
                }
            }
            else
            {
                PlayerPrefs.SetString("ClipsPaths", "");
            }
    
        }
    
        public void SearchYesutton()
        {
            AudioManager.Instance.UiPlay((AudioManager.UiSound)UnityEngine.Random.Range(0, 5));
            GetComponent<MusicUICanvas>().SetPageUI(MusicUICanvas.MusicPage.SearchMusic);
            StartCoroutine("SearchMusic");
        }
    
        // 텍스트와 함께 검색
        IEnumerator SearchMusic()
        {
            // 커스텀 클립 없애기
            DeleteCustomClipList();
    
            // 지금까지의 리스트 제거
            rootPaths.Clear();
            clipPaths.Clear();
            clips.Clear();
            customPlusClips.Clear();
    
            // 리스트 작성중 text 활성화
            searchMusicText.gameObject.SetActive(true);
    
            #region 루트폴더 검색
    #if UNITY_ANDROID
            // 만약 안드로이드라면
            searchMusicText.UID = "SEARCHFOLDER";
            yield return new WaitForSecondsRealtime(1f);
    
            // 길이만큼 검색
            for (int i = 0; i < androidRootlDirs.Length; i++)
            {
                // 존재한다면
                if (Directory.Exists(androidRootlDirs[i]))
                {
                    // 존재하는 파일경로 저장
                    rootPaths.Add(androidRootlDirs[i]);
                }
            }
            
    #endif
    #if UNITY_EDITOR
    
            searchMusicText.UID = "SEARCHFOLDER";
            yield return new WaitForSecondsRealtime(1f);
    
            for (int i = 0; i < windowRootlDirs.Length; i++)
            {
                if (Directory.Exists(windowRootlDirs[i]))
                {
                    rootPaths.Add(windowRootlDirs[i]);
                }
            }
            
    #endif
            #endregion
    
            searchMusicText.UID = "SEARCHCLIP";
            yield return new WaitForSecondsRealtime(1f);
    
    #region 서브폴더의 클립 저장
            // 루트 폴더의 크기 지정
            rootFolder = new DirectoryInfo[rootPaths.Count];
    
            // 루트 패스의 숫자만큼 반복
            for (int i = 0; i < rootPaths.Count; i++)
            {
                // string으로 된 Path를 디렉토리인포로 변환하여 저장
                rootFolder[i] = new DirectoryInfo(rootPaths[i]);
    
                // 파일의 타입만큼 반복
                for (int j = 0; j < fildTypes.Length; j++)
                {
    
                    // 서브파일을 저장
                    subFile = rootFolder[i].GetFiles(fildTypes[j], SearchOption.AllDirectories);
    
                    // 서브파일의 갯수만큼 반복
                    foreach (FileInfo file in subFile)
                    {
                        // 저장된 클립패스 수만큼 반복
                        for (int k = 0; k < clipPaths.Count; k++)
                        {
                            // 파일의 이름이
                            // clips[].name 중에 같은 것이 없을 때만
                            if (file.Name == Path.GetFileName(clipPaths[k]))
                            {
                                isTwice = true;
                                break;
                            }
                        }
    
                        // 중복이 아닐때만 저장
                        if (!isTwice)
                        {
                            // 서브 폴더에 있는 파일경로값을 저장
                            clipPaths.Add(file.FullName);
    
                            searchMusicText.UID = "SEARCHED";
                            searchMusic.text = Path.GetFileName(clipPaths[clipPaths.Count - 1]);
                            yield return new WaitForSecondsRealtime(0);
                        }
    
                        // 초기화
                        isTwice = false;
                        yield return new WaitForSecondsRealtime(0);
                    }
                    searchMusicText.UID = "SEARCHING";
                    searchMusic.text = "";
                    yield return new WaitForSecondsRealtime(0);
                }
                yield return new WaitForSecondsRealtime(0);
            }
    #endregion
    
            // 플레이어 프랩스에 저장
            ClipsPathSave();
    
            if (clipPaths.Count <= 0)
            {
                searchMusicText.UID = "SEARCHDONE";
                searchMusic.text = "" + clipPaths.Count;
                yield return new WaitForSecondsRealtime(1f);
                searchMusic.text = "";
            }
            else
            {
                searchMusicText.UID = "SEARCHDONE";
                searchMusic.text = "" + clipPaths.Count;
                yield return new WaitForSecondsRealtime(1f);
                searchMusicText.UID = "SEARCHCREATE";
                searchMusic.text = "";
            }
            yield return new WaitForSecondsRealtime(1f);
    
            // UI
            GetComponent<MusicUICanvas>().SetPageUI(MusicUICanvas.MusicPage.CustomList);
    
            // 클립 리스트 설정
            CheckCustomClipList();
    
            // 리스트 작성중 text 비활성화
            searchMusicText.gameObject.SetActive(false);
            searchMusic.gameObject.SetActive(false);
    
            // 다시 일시정지 가능
            GameManager.Instance.youCanPauseEsc = true;
        }
    
        // 클립의 패스 저장하기(PlayerPrefs)
        void ClipsPathSave()
        {
            tempString = "";
            // 저장하기
            // 클립의 full.Name를 저장해놓자
            // 클립의 갯수만큼 반복
            for (int i = 0; i < clipPaths.Count; i++)
            {
                // 클립의 임시string += full.Name
                tempString += clipPaths[i];
                // 마지막 거엔 쉼표를 붙이지 않는다.
                if (i < clipPaths.Count - 1)
                {
                    // 쉼표를 붙인다
                    tempString += ",";
                }
            }
    
            PlayerPrefs.SetString("ClipsPaths", tempString);
        }
    
        // 디폴트 클립 리스트 만들기
        public void CheckDefaultClipList()
        {
            // clipButtonPool를 활성화
            for (int i = 0; i < defaultClipButtonPool.Length; i++)
            {
                defaultClipButtonPool[i].SetActive(true);
                // 이름 바꿔주기
                defaultClipButtonPool[i].GetComponentInChildren<Text>().text = AudioManager.Instance.bgmClips[i].name;
                // 클립 패스 넘겨주기
                defaultClipButtonPool[i].GetComponentInChildren<ClipSelectButton>().musicClip = AudioManager.Instance.bgmClips[i];
            }
    
            // 비활성화
            // 만약 clipListCount의 숫자가 clips.Count보다 작다면
            if (clipListCount >= clipPaths.Count)
            {
                // clips.Count만큼 clipButtonPool를 활성화
                for (int i = 0; i < clipPaths.Count; i++)
                {
                    customClipButtonPool[i].SetActive(false);
                }
            }
            // 만약 클립의 수가 배열 수를 넘는다면
            else if (clipListCount < clipPaths.Count)
            {
                // 모든 clipButtonPool를 활성화
                for (int i = 0; i < clipListCount; i++)
                {
                    customClipButtonPool[i].SetActive(false);
                }
                // 추가적으로 생성 (clips.Count - clipListCount)된 애들은 지워준다
                for (int i = 0; i < customPlusClips.Count; i++)
                {
                    Destroy(customPlusClips[i]);
                }
            }
    
        }
        // 커스텀 클립 리스트 만들기
        public void CheckCustomClipList()
        {
            // 만약 clipListCount의 숫자가 clips.Count보다 작다면
            // 클립 패스의 갯수가 0이 아니라면
            if (clipListCount >= clipPaths.Count && clipPaths.Count != 0)
            {
                // clips.Count만큼 clipButtonPool를 활성화
                for (int i = 0; i < clipPaths.Count; i++)
                {
    
                    customClipButtonPool[i].SetActive(true);
                    // 이름 바꿔주기
                    customClipButtonPool[i].GetComponentInChildren<Text>().text = Path.GetFileName(clipPaths[i]);
                    // 클립 패스 넘겨주기
                    customClipButtonPool[i].GetComponentInChildren<ClipSelectButton>().musicPath = clipPaths[i];
                }
            }
            // 만약 클립의 수가 배열 수를 넘는다면
            else if (clipListCount < clipPaths.Count)
            {
                // 모든 clipButtonPool를 활성화
                for (int i = 0; i < clipListCount; i++)
                {
                    customClipButtonPool[i].SetActive(true);
                    // 이름 바꿔주기
                    customClipButtonPool[i].GetComponentInChildren<Text>().text = Path.GetFileName(clipPaths[i]);
                    // 클립값 넘겨주기
                    customClipButtonPool[i].GetComponentInChildren<ClipSelectButton>().musicPath = clipPaths[i];
                }
                // 추가적으로 생성 (clips.Count - clipListCount)
                for (int i = clipListCount; i < clipPaths.Count; i++)
                {
                    GameObject clipButtonPlus = Instantiate(customClipButtonFactory, gridImage);
                    clipButtonPlus.SetActive(true);
                    // 추가된 아이들 리스트에 넣어주기
                    customPlusClips.Add(clipButtonPlus);
                    // 이름 바꿔주기
                    clipButtonPlus.GetComponentInChildren<Text>().text = Path.GetFileName(clipPaths[i]);
                    // 클립값 넘겨주기
                    clipButtonPlus.GetComponentInChildren<ClipSelectButton>().musicPath = clipPaths[i];
                }
            }
    
            // clipButtonPool를 비활성화
            for (int i = 0; i < defaultClipButtonPool.Length; i++)
            {
                defaultClipButtonPool[i].SetActive(false);
            }
    
        }
    
        // 디폴트 클립 없애기
        public void DeleteDefaultClipList()
        {
            // 모든 clipButtonPool를 비활성화
            for (int i = 0; i < defaultClipButtonPool.Length; i++)
            {
                defaultClipButtonPool[i].SetActive(false);
            }
        }
        // 커스텀 클립 없애기
        public void DeleteCustomClipList()
        {
            // 모든 clipButtonPool를 비활성화
            for (int i = 0; i < customClipButtonPool.Length; i++)
            {
                customClipButtonPool[i].SetActive(false);
            }
            // 추가적으로 생성 (clips.Count - clipListCount)된 애들은 지워준다
            for (int i = 0; i < customPlusClips.Count; i++)
            {
                Destroy(customPlusClips[i]);
            }
            
        }
    }
    ```
	</details>
	
## 📸 Capture System

스크린샷과 기기의 카메라를 제어하는 시스템입니다.
{:.message}

- 안드로이드 플러그인을 이용하여 갤러리에 스크린샷을 저장합니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>   
	```csharp
	using System.Collections;
	using System.Collections.Generic;
	using System.IO;
	using UnityEngine;
	
	public class GetPicture : MonoBehaviour {
	
	#if UNITY_ANDROID
		private static AndroidJavaClass m_ajc = null;
		private static AndroidJavaClass AJC
		{
			get
			{
				if (m_ajc == null)
					m_ajc = new AndroidJavaClass("com.yasirkula.unity.NativeGallery");
	
				return m_ajc;
			}
		}
	#endif
	
		public static string GetLastPicturePath()
		{
			List<string> shotFiles = new List<string>();
			string saveDir;
	#if UNITY_ANDROID
			saveDir = AJC.CallStatic<string>( "GetMediaPath", "Shine Bright" );
	#else
			saveDir = Application.persistentDataPath;
	#endif
			string[] files = Directory.GetFiles(saveDir, "*.png");
	
			for (int i = 0; i < files.Length; i++)
			{
				if (files[i].Contains("ShineBrightScreenshot_"))
				{
					shotFiles.Add(files[i]);
				}
			}
	
			if (shotFiles.Count > 0)
			{
				return shotFiles[shotFiles.Count - 1];
			}
			return null;
		}
	}
	```
        
	```csharp
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	using System.IO;
	using UnityEngine.UI;
	
	public class CameraUICanvas : MonoBehaviour
	{
		public GameObject blink;             // 사진 찍을 때 깜빡일 것
		public GameObject shareButtons;      // 공유 버튼
	
		bool isCoroutinePlaying;             // 코루틴 도중엔 사진이 찍히지 않도록 함
	
		// 파일 불러올 때 필요
		string[] files;                      // PNG파일들 경로 저장
		string albumName = "Shine Bright";   // 생성될 앨범의 이름
		[SerializeField]
		GameObject panel;                    // 찍은 사진이 뜰 패널
	
		bool firstCount;                     // 첫번째 실행인지 체크하기 위한 변수
	
		private void OnEnable()
		{
			transform.Find("Panel").gameObject.SetActive(false);
		}
	
		public void Captureutton()
		{
			if (!isCoroutinePlaying)
			{
				StartCoroutine("captureScreenshot");
				GameManager.Instance.youCanPauseEsc = false;
				// 업적이 달성되어있지 않으면 최초 실행시 달성
	
				// 만일 처음 셔터를 눌렀을 때
				if (!firstCount)
				{
					// 업적이 달성되어있지 않으면 달성시키고 아니면 종료한다
					GameCenterManager.Instance.SuccessDeviceAchievement(GameCenterManager.MyAchievement.ClickTheShutter);
					firstCount = true;
				}
			}
		}
	
		IEnumerator captureScreenshot()
		{
			isCoroutinePlaying = true;
	
			// UI 없앤다!
			for (int i = 0; i < transform.childCount; i++)
			{
				transform.GetChild(i).gameObject.SetActive(false);
			}
			GameObject moneyCanvas = GameObject.Find("MoneyCanvas");
			if (moneyCanvas != null)
			{
				moneyCanvas.SetActive(false);
			}
	
			yield return new WaitForEndOfFrame();
	
			// 스크린샷 + 갤러리갱신
			ScreenshotAndGallery();
	
			yield return new WaitForEndOfFrame();
	
			// 블링크
			BlinkUI();
	
			// 셔터 사운드
			AudioManager.Instance.UiPlay((AudioManager.UiSound)Random.Range(5, 8));
	
			yield return new WaitForEndOfFrame();
	
			// UI 다시 나온다
			ShowAgainUI();
	
			// 찍은 사진이 등장
			GetPirctureAndShowIt();
	
			isCoroutinePlaying = false;
		}
	
		// 흰색 블링크 생성
		void BlinkUI()
		{
			GameObject b = Instantiate(blink);
			b.transform.SetParent(transform);
			b.transform.localPosition = new Vector3(0, 0, 0);
			b.transform.localScale = new Vector3(1, 1, 1);
		}
	
		// 스크린샷 찍고 갤러리에 갱신
		void ScreenshotAndGallery()
		{
			// 스크린샷
			Texture2D ss = new Texture2D(Screen.width, Screen.height, TextureFormat.RGB24, false);
			ss.ReadPixels(new Rect(0, 0, Screen.width, Screen.height), 0, 0);
			ss.Apply();
	
			// 갤러리갱신
			Debug.Log("" + NativeGallery.SaveImageToGallery(ss, albumName,
				"ShineBrightScreenshot_" + System.DateTime.Now.ToString("dd-MM-yyyy-HH-mm-ss") + "{0}.png"));
	
			// To avoid memory leaks.
			// 복사 완료됐기 때문에 원본 메모리 삭제
			Destroy(ss);
		}
	
		// 찍은 사진을 Panel에 보여준다.
		void GetPirctureAndShowIt()
		{
			string pathToFile = GetPicture.GetLastPicturePath();
			if (pathToFile == null)
			{
				return;
			}
			Texture2D texture = GetScreenshotImage(pathToFile);
			Sprite sp = Sprite.Create(texture, new Rect(0, 0, texture.width, texture.height), new Vector2(0.5f, 0.5f));
			panel.SetActive(true);
			shareButtons.SetActive(true);
			panel.GetComponent<Image>().sprite = sp;
		}
		// 찍은 사진을 불러온다.
		Texture2D GetScreenshotImage(string filePath)
		{
			Texture2D texture = null;
			byte[] fileBytes;
			if (File.Exists(filePath))
			{
				fileBytes = File.ReadAllBytes(filePath);
				texture = new Texture2D(2, 2, TextureFormat.RGB24, false);
				texture.LoadImage(fileBytes);
			}
			return texture;
		}
	
		void ShowAgainUI()
		{
			// UI 다시 나온다
			for (int i = 0; i < transform.childCount; i++)
			{
				transform.GetChild(i).gameObject.SetActive(true);
			}
			transform.Find("Panel").gameObject.SetActive(false);
		}
	
	}
	```
	</details>
- 기기의 후면카메라를 불러와 오버레이합니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
    ```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.UI;
    
    public class RearCam : MonoBehaviour {
    
        bool camAvailabe;
        WebCamDevice[] devices;
    
        WebCamTexture backCam;
    
        float scaleY;
        int orient;
    
        public void DetectCamera()
        {
            // 카메라에 맞추기
            Camera cam = Camera.main;
            float pos = (cam.nearClipPlane + 17.5f);
            transform.position = cam.transform.position + cam.transform.forward * pos;
            float h = Mathf.Tan(cam.fieldOfView * Mathf.Deg2Rad * 0.5f) * pos * 2f;
            transform.localScale = new Vector3(h * cam.aspect, h, 0f);
    
            transform.SetParent(Camera.main.transform);
    
            // 디바이스 검색
            devices = WebCamTexture.devices;
    
            // 카메라가 있는지 검색
            if (devices.Length == 0)
            {
                camAvailabe = false;
                return;
            }
    
            // 디바이스 중에 후면 카메라가 있는지 검사
            for (int i = 0; i < devices.Length; i++)
            {
                if (!devices[i].isFrontFacing)
                {
                    // 스크린 width와 height 크기에 맞게 다시 갱신해줄 것
                    backCam = new WebCamTexture(WebCamTexture.devices[0].name, Screen.width, Screen.height);
                }
            }
    
            // 후면 카메라가 없다
            if (backCam == null)
            {
                camAvailabe = false;
                return;
            }
    
            GetComponent<Renderer>().material.mainTexture = backCam;
    
            // 있다면 플레이
            if (!backCam.isPlaying)
                backCam.Play();
    
            camAvailabe = true;
        }
    
        public void CameraActivate()
        {
            if (!camAvailabe)
            {
                gameObject.SetActive(false);
                return;
            }
    
            // 거울반전
            scaleY = backCam.videoVerticallyMirrored ? -1f : 1f;
            transform.localScale = new Vector3(transform.localScale.x, transform.localScale.y * scaleY, transform.localScale.z);
            // 상하 돌리기
            orient = -backCam.videoRotationAngle;
            transform.localEulerAngles = new Vector3(0, 0, orient);
        }
    }
    ```
	</details>
	
## ✨ Ads System

광고와 인게임 재화를 관리하는 시스템입니다.
{:.message}

- 유니티 Ads를 활용하여 광고를 보면 인게임 재화를 지급합니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
    ```csharp
    using UnityEngine;
    #if UNITY_ADS
    using UnityEngine.Advertisements;
    #endif
    
    public class AdsManager : MonoBehaviour
    {
        // - 보상형 광고 카운트
        int adsRewardCount = 0;
        int AdsCount
		{
            get
			{
                return PlayerPrefs.GetInt("ADCOUNT", adsRewardCount);
            }
            set
			{
                adsRewardCount = value;
                if(adsRewardCount == 5)
				{
                    // 보상형광고를 5번 보게 되면 업적 완료
                    GameCenterManager.Instance.SuccessDeviceAchievement(GameCenterManager.MyAchievement.ThankYouSoMuch);
                    PlayerPrefs.SetInt("ADCOUNT", adsRewardCount);
                }
            }
        }
    
    	// 광고를 보여준다.
    	public void ShowDefaultAd()
        {
    #if UNITY_ADS
            if (!Advertisement.IsReady())
            {
                Debug.Log("Ads not ready for default placement");
                return;
            }
           
            Advertisement.Show();
    #endif
        }
    
        public void ShowRewardedAd()
        {
            const string RewardedPlacementId = "rewardedVideo";
    
    #if UNITY_ADS
            if (!Advertisement.IsReady(RewardedPlacementId))
            {
                Debug.Log(string.Format("Ads not ready for placement '{0}'", RewardedPlacementId));
                return;
            }
    
            var options = new ShowOptions { resultCallback = HandleShowResult };
            Advertisement.Show(RewardedPlacementId, options);
    #endif
        }
    
    #if UNITY_ADS
    
        // 광고를 봤는지에 대한 결과 여부의 정보를 가져온다.
        private void HandleShowResult(ShowResult result)
        {
            switch (result)
            {
                case ShowResult.Finished:
                    Debug.Log("The ad was successfully shown.");
                    // - 광고 시청 완료 시 팝업
                    GameObject.Find("MoneyCanvas").GetComponent<Animator>().SetBool("MoneyEarn", true);
                    // - 광고 시청 횟수 카운트
                    AdsCount++;
                    // - Player 에게 별 10개 추가
                    MoneyManager.Instance.Combo += 10;
                    break;
                case ShowResult.Skipped:
                    Debug.Log("The ad was skipped before reaching the end.");
                    // - Skipped : 광고를 스킵한 경우
                    break;
                case ShowResult.Failed:
                    Debug.LogError("The ad failed to be shown.");
                    // - Failed : 어떤 이유 때문에 광고 시청에 실패한 경우
                    break;
            }
        }
    
    #endif
    }
    ```
	</details>
- 게임재화를 이용하여 테마를 구매하고 적용합니다.

## 🆎 Localizing System

다국어 대응을 위한 시스템입니다.
{:.message}

- Enum으로 국가를 설정하고, 
Dictionary로 Key값에 대응하는 Text를 CSV에서 호출하는 방식으로 구현했습니다.
- 첫 시작 시 Application.systemLanguage를 이용하여 기기에 맞는 언어로 설정하였습니다.
- PlayerPrefs로 옵션을 저장하였습니다.

## 🔃 Sharing System

SNS 공유를 위한 시스템입니다.
{:.message}

- 안드로이드 플러그인을 이용하여 공용 공유 시스템을 구축하였습니다.
- 트위터에서 제공하는 sdk를 이용하여 해당 SNS에 바로 접근 가능하도록 구축하였습니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
    ```csharp
    using UnityEngine;
    using System.Collections;
    using TwitterKit.Unity;
    using System.IO;
    
    public class TwitterManager : MonoBehaviour
    {
    
        string frontStr;
        string hashtagStr = "#ShineBright";
    
        public void startLogin()
        {
            UnityEngine.Debug.Log("startLogin");
            // API 키 설정
            Twitter.Init();
    
            Twitter.LogIn(LoginCompleteWithCompose, (ApiError error) =>
            {
                Debug.Log(error.message);
            });
        }
    
        public void LoginCompleteWithEmail(TwitterSession session)
        {
            // 이메일 인증
            UnityEngine.Debug.Log("LoginCompleteWithEmail");
            Twitter.RequestEmail(session, RequestEmailComplete, (ApiError error) => { UnityEngine.Debug.Log(error.message); });
        }
    
        public void RequestEmailComplete(string email)
        {
            UnityEngine.Debug.Log("email=" + email);
            LoginCompleteWithCompose(Twitter.Session);
        }
    
        public void LoginCompleteWithCompose(TwitterSession session)
        {
            if (LanguageManager.nowLoc == Location.Korean)
            {
                frontStr = "Download: ";
            }
            else if (LanguageManager.nowLoc == Location.Japanese)
            {
                frontStr = "Download: ";
            }
            else
            {
                frontStr = "Download: ";
            }
            string imageUri = "file://" + GetPicture.GetLastPicturePath();
            Twitter.Compose(session, imageUri, frontStr, new string[] { hashtagStr },
                (string tweetId) => { UnityEngine.Debug.Log("Tweet Success, tweetId=" + tweetId); },
                (ApiError error) => { UnityEngine.Debug.Log("Tweet Failed " + error.message); },
                () => { Debug.Log("Compose cancelled"); }
             );
        }
    }
    ```
	</details>
## 🍧 GameCenter System

구글 플레이와의 연동을 위한 시스템입니다.
{:.message}

- 로그인: 로그인 시에 업적 달성여부를 불러와 적용합니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
	```csharp
	// 로그인 팝업
	public void SignIn()
	{
		if (Social.localUser.authenticated == false)
		{
			Social.localUser.Authenticate((bool success) =>
			{
				if (success)
				{
					// Sign In 성공
				}
				else
				{
					// Sign In 실패 처리
					return;
				}
			});
		}

	}
	```
	</details>
        
- 업적 시스템: 업적을 달성할 때마다 인게임 재화를 지급합니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
	```csharp
	// 업적 UI 표시
	public void ShowAchievementUI()
	{
		// Sign In 이 되어있지 않은 상태라면
		// Sign In 후 업적 UI 표시 요청할 것
		if (Social.localUser.authenticated == false)
		{
			Social.localUser.Authenticate((bool success) =>
			{
				if (success)
				{
					// Sign In 성공
					// 구글 업적 달성
					CompleteAhievement();
					// 바로 업적 UI 표시 요청
					Social.ShowAchievementsUI();
					return;
				}
				else
				{
					// Sign In 실패 처리
					return;
				}
			});
		}

		// 구글 업적 달성
		CompleteAhievement();
		Social.ShowAchievementsUI();
	}
	
	public void CheckAchievement()
	{
		// 로그인
		SignIn();

		Social.LoadAchievements(achievements =>
		{
			// 1. 만일 업적이 존재한다면 
			if (achievements.Length > 0)
			{
				// 2. 존재하는 업적을 검사한다
				for (int i = 0; i < achievements.Length; i++)
				{
					RewardCanvas.Instance.rewardBools[i] = achievements[i].completed;
				}
			}
		});
	}
	// 업적 리스트 가져오기 
	void GetMyAchievment()
	{
		Social.LoadAchievements(achievements =>
		{
			if (achievements.Length > 0)
			{
				Debug.Log("Got " + achievements.Length + " achievement instances");
				string myAchievements = "My achievements:\n";
				foreach (IAchievement achievement in achievements)
				{
					myAchievements += "\t" +
						achievement.id + " " +
						achievement.percentCompleted + " " +
						achievement.completed + " " +
						achievement.lastReportedDate + "\n";
				}
				Debug.Log(myAchievements);
			}
			else
				Debug.Log("No achievements returned");
		});
	}
	
	public void CompleteAhievement(string id)
	{
	#if UNITY_ANDROID
		// ReportProgress("업적ID", 업적 진행도 0f~100f(단순조건충족은 100), callback);
		PlayGamesPlatform.Instance.ReportProgress(id, 100f, null);
	#elif UNITY_IOS
		Social.ReportProgress("Reward ID", 100f, null);
	#endif
	}
	```
	</details>
- 리더보드 시스템: 구글 플레이 내에서의 랭크를 기록합니다.
- 리더보드: 게임오버 시에 BestScore를 리더보드에 랭킹을 저장합니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>
    ```csharp
    string leaderBoardID = "~";
    
    public void RankButton()
    {
    	PlayGamesPlatform.Activate();
    	Social.localUser.authenticate(AuthenticateHandler);
    }
    
    void AuthenticateHandler(bool isSuccess)
	{
		if (isSuccess)
		{
			float highScore = PlayerPrefs.GetFloat("TopScore", ScoreManager.Instance.topScore);
			Social.ReportScore((long)highScore, leaderBoardID, (bool success) =>
			{
				if (success)
				{
					PlayGamesPlatform.Instance.ShowLeaderboardUI(leaderBoardID);
				}
				else
				{
					// 점수 저장 실패
				}
			});
		}
		else
		{
			// 로그인 실패
		}
	}
    ```
	</details>