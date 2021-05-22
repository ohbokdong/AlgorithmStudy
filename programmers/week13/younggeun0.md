# [방금 그 곡](https://programmers.co.kr/learn/courses/30/lessons/17683)

```js
function getPlayTime(startTime, endTime) {
    startTime = startTime.split(":");
    endTime = endTime.split(":");

    var h = parseInt(endTime[0]) - parseInt(startTime[0]);
    var m = parseInt(endTime[1]) - parseInt(startTime[1]);

    if (h != 0) {
        // m = (60 * h) - m; 빼는게 아님 ㅠㅠ
        m = (60 * h) + m; // 19, 27 틀렸던건 재생시간 계산이 잘못됨..
    }
    return m;
}

function getChords(str) {
    // 문자열 코드를 배열로 변환
    var chords = [];
    var idx = 0;
    var chord = "";

    while (idx < str.length) {
        if (str[idx+1] == "#") {
            chord = str.substring(idx, idx+2);
            idx += 2;
        } else {
            chord = str.substring(idx, idx+1);   
            idx++;
        }
        chords.push(chord);
    }
    return chords;
}
function solution(m, musicinfos) {
    var answer = '';
    // 끝과 처음이 이어지거나, 중간에 끊을 경우가 존재
    // m - 기억한 멜로디를 담은 문자열 (1<= m <= 1439)
    // musicinfos - 100개 이하의 곡 정보를 담은 배열
    // 곡 정보 - 콜론으로 구분된 문자열
    // "음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보"
    // - 시작, 끝난 시각은 24시간 HH:MM 형식
    // - 제목은 ',' 이외의 출력 가능한 문자로 표현된 길이 1이상 64이하 문자열
    // - 악보 정보는 음 1개 이상 1439개 이하로 구성

    var theSongs = [];
    // 1. musicinfos에서 코드 수 == 음악 길이, 재생 시간을 계산해 코드 문자열을 재가공
    for (var i=0; i<musicinfos.length; i++) {
        var infos = musicinfos[i].split(',');
        var startTime = infos[0];
        var endTime = infos[1];
        var playtime = getPlayTime(startTime, endTime);
        var songName = infos[2];
        var chords = getChords(infos[3]);

        var chordsLength = chords.length;
        var chordIdx = 0;
        var music = [];
        for (var j=0; j<playtime; j++) {
            music.push(chords[chordIdx++]);
            
            if (chordIdx > chordsLength - 1) {
                chordIdx = 0;
            }
        }

        // 2. 재가공된 코드배열 속에 m코드들이 포함되는지 확인
        var melody = getChords(m);
        var bTheSong = false;
        for (var musicIdx=0; musicIdx < music.length; musicIdx++) {
            if (music[musicIdx] == melody[0] && music[musicIdx + (melody.length - 1)]) {                
                bTheSong = true;
                for (var melodyIdx=0; melodyIdx < melody.length; melodyIdx++) {
                    if (music[musicIdx + melodyIdx] != melody[melodyIdx]) {
                        bTheSong = false;
                        break;
                    }

                    if (melodyIdx == (melody.length - 1) && bTheSong) {
                        var songInfo = {
                            songName: songName,
                            playtime: playtime
                        };
                        theSongs.push(songInfo);
                        musicIdx += (melody.length - 1);
                        break;
                    }
                }
            }
        }
    }    
    
    if (theSongs.length > 1) {
        // 조건이 일치하는 음악이 여러 개 일 때 재생시간이 제일 긴 음악 제목을 반환
        var nLogest = 0;
        for (var i=0; i<theSongs.length; i++) {
            var songInfo = theSongs[i];
            var playtime = songInfo.playtime;
            if (playtime > nLogest) {
                nLogest = playtime;
                answer = songInfo.songName;
            }
        }
    } else if (theSongs.length == 1){
        answer = theSongs[0].songName;
    } else {
        answer = "(None)";
    }
        
    return answer;
}

function solution(m, musicinfos) {
    var answer = '';
    // 끝과 처음이 이어지거나, 중간에 끊을 경우가 존재
    // m - 기억한 멜로디를 담은 문자열 (1<= m <= 1439)
    // musicinfos - 100개 이하의 곡 정보를 담은 배열
    // 곡 정보 - 콜론으로 구분된 문자열
    // "음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보"
    // - 시작, 끝난 시각은 24시간 HH:MM 형식
    // - 제목은 ',' 이외의 출력 가능한 문자로 표현된 길이 1이상 64이하 문자열
    // - 악보 정보는 음 1개 이상 1439개 이하로 구성

    var theSongs = [];
    // 1. musicinfos에서 코드 수 == 음악 길이, 재생 시간을 계산해 코드 문자열을 재가공
    for (var i=0; i<musicinfos.length; i++) {
        var infos = musicinfos[i].split(',');
        var startTime = infos[0];
        var endTime = infos[1];
        var playtime = getPlayTime(startTime, endTime);
        var songName = infos[2];
        var chords = getChords(infos[3]);

        var chordsLength = chords.length;
        var chordIdx = 0;
        var music = [];
        for (var j=0; j<playtime; j++) {
            music.push(chords[chordIdx++]);
            
            if (chordIdx > chordsLength - 1) {
                chordIdx = 0;
            }
        }

        // 2. 재가공된 코드배열 속에 m코드들이 포함되는지 확인
        var melody = getChords(m);
        var bTheSong = false;
        for (var musicIdx=0; musicIdx < music.length; musicIdx++) {
            if (music[musicIdx] == melody[0] && music[musicIdx + (melody.length - 1)]) {                
                bTheSong = true;
                for (var melodyIdx=0; melodyIdx < melody.length; melodyIdx++) {
                    if (music[musicIdx + melodyIdx] != melody[melodyIdx]) {
                        bTheSong = false;
                        break;
                    }

                    if (melodyIdx == (melody.length - 1) && bTheSong) {
                        var songInfo = {
                            songName: songName,
                            playtime: playtime
                        };
                        theSongs.push(songInfo);
                        musicIdx += (melody.length - 1);
                        break;
                    }
                }
            }
        }
    }    
    
    if (theSongs.length > 1) {
        // 조건이 일치하는 음악이 여러 개 일 때 재생시간이 제일 긴 음악 제목을 반환
        var nLogest = 0;
        for (var i=0; i<theSongs.length; i++) {
            var songInfo = theSongs[i];
            var playtime = songInfo.playtime;
            if (playtime > nLogest) {
                nLogest = playtime;
                answer = songInfo.songName;
            }
        }
    } else if (theSongs.length == 1){
        answer = theSongs[0].songName;
    } else {
        answer = "(None)";
    }
        
    return answer;
}
```

* ~~19, 27번 테스트케이스만 실패한다.. 뭐가 문젤까..~~ => 재생시간 계산 오류 
