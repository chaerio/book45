# book45 oracle 

CREATE TABLE BOOK(

    NUM NUMBER(5), 
    ISBN NUMBER(38) PRIMARY KEY,
    CATEGORY NVARCHAR2(1000),
    TITLE NVARCHAR2(1000), 
    PRICE NUMBER(10), 
    SUMMARY VARCHAR2(4000),
    AUTHOR NVARCHAR2(1000), 
    PUB NVARCHAR2(1000),
    GRADE NVARCHAR2(10),
    PICTUREURL VARCHAR2(4000),
    STOCK NUMBER(10) DEFAULT 10,
    RATINGAVG NUMBER(3,1)
    
);

CREATE SEQUENCE BOOK45_SEQ;

----------------------------------------------------------------------------------------

CREATE TABLE ALBUM (

    NUM NUMBER(5),
    PRODUCTNUM NUMBER(38) PRIMARY KEY,
    CATEGORY VARCHAR2(3000),
    ALBUMTITLE VARCHAR2(3000),
    ALBUMPRICE NUMBER(10),
    SINGER VARCHAR2(3000),
    ENT VARCHAR2(3000),
    RELEASEDATE VARCHAR2(3000),
    ALBUMPICTUREURL VARCHAR2(4000),
    STOCK NUMBER(5) DEFAULT 10,
    RATINGAVG NUMBER(3,1)
    
);


CREATE SEQUENCE ALBUM_SEQ;
 
commit;

----------------------------------------------------------------------------------------

CREATE TABLE BOOKREVIEW(

    NUM NUMBER(10) PRIMARY KEY NOT NULL,
    ISBN NUMBER(38) CONSTRAINT FK_ISBN REFERENCES BOOK(ISBN) ON DELETE CASCADE,
    ID VARCHAR2(20) CONSTRAINT FK_BOOKREVIEWID REFERENCES MEMBER(ID) ON DELETE CASCADE NOT NULL,
    NICKNAME VARCHAR2(30) NULL,
    CONTENT VARCHAR2(4000) NULL,
    RATING  NUMBER(3,1) NOT NULL, 
    WRITEDATE DATE DEFAULT SYSDATE
    
);

CREATE SEQUENCE BOOKREVIEW_SEQ;

select * from book;
select * from bookReview;

insert into bookreview(num, isbn, id, nickname, content, rating)
values(bookreview_seq.nextval, 9788954699914, 'admin', '가상닉네임', '테스트', 1);
    
----------------------------------------------------------------------------------------

CREATE TABLE ALBUMREVIEW(

    NUM NUMBER(10) PRIMARY KEY,
    PRODUCTNUM NUMBER(38) CONSTRAINT FK_PRODUCTNUM REFERENCES ALBUM(PRODUCTNUM) ON DELETE CASCADE NOT NULL,
    ID VARCHAR2(20) CONSTRAINT FK_ALBUMREVIEWID REFERENCES MEMBER(ID) ON DELETE CASCADE NOT NULL,
    NICKNAME VARCHAR2(30) NOT NULL,
    CONTENT VARCHAR2(4000),
    RATING NUMBER(3,1) NOT NULL,
    WRITEDATE DATE DEFAULT SYSDATE
    
);

CREATE SEQUENCE ALBUMREVIEW_SEQ;

commit;

select * from albumReview;

----------------------------------------------------------------------------------------

CREATE TABLE MEMBER(

    ID VARCHAR2(20) PRIMARY KEY,
    PASS VARCHAR2(20) NOT NULL,
    NAME VARCHAR2(20),
    NICKNAME VARCHAR2(20),
    PHONE VARCHAR2(20),
    EMAIL VARCHAR2(50),
    ZIPCODE VARCHAR2(10),
    ADDRESSROAD VARCHAR2(1000),
    ADDRESSDETAIL VARCHAR2(2000),
    LEV CHAR(1) DEFAULT 'B', -- A: 관리자, B: 일반회원
    POINT NUMBER(10)
    
);
    
commit;

select * from member;

insert into member(id, pass, name, nickname, phone, email, zipcode, addressroad, addressdetail, lev, point) 
values('admin',1234, '관리자','관리자닉네임','010-9189-3763','cofl0104@naver.com','16585','경기도 수원시 권선구','경수대로','A',10);
insert into member(id, pass, name, nickname, phone, email, zipcode, addressroad, addressdetail, lev, point) 
values('member',1234, '일반회원','일반닉네임','010-0000-0000','cofl0000@naver.com','0000','경기도 수원시 권선구2','경수대로2','B',1);
insert into member(id, pass, name, nickname, phone, email, zipcode, addressroad, addressdetail, lev, point) 
values('cofl000',1234, '임채리','채리쨩','010-1111-0000','cofl0000@naver.com','0000','경기도 수원시 권선구2','경수대로2','B',1);

----------------------------------------------------------------------------------------
CREATE TABLE BOARD (
    NUM NUMBER(10) PRIMARY KEY,
    MEMBERID VARCHAR2(20) CONSTRAINT BOARD_ID_FK REFERENCES MEMBER(ID) ON DELETE CASCADE,
    PASS VARCHAR2(20) NOT NULL,
    TITLE VARCHAR2(1000) NOT NULL,
    CONTENT VARCHAR2(4000) NOT NULL,
    WRITEDATE DATE DEFAULT SYSDATE,
    UPDATEDATE DATE DEFAULT SYSDATE,
    READCOUNT NUMBER(30) DEFAULT 0,
    REPLYCNT NUMBER(30) DEFAULT 0,
    BLIND CHAR(10) DEFAULT 0
);

CREATE SEQUENCE BOARD_SEQ;

select * from board order by num desc;

commit;

----------------------------------------------------------------------------------------
CREATE TABLE REPLY (
    REPLYNUM NUMBER(10) PRIMARY KEY,
    BOARDNUM NUMBER(10) CONSTRAINT FK_BOARD_NUM REFERENCES BOARD(NUM) ON DELETE CASCADE,
    MEMBERID VARCHAR2(20) CONSTRAINT FK_MEMBER REFERENCES MEMBER(ID) ON DELETE CASCADE,
    NICKNAME VARCHAR2(30) NOT NULL,
    CONTENT VARCHAR2(4000) NOT NULL,
    REPLYDATE DATE DEFAULT SYSDATE,
    SECRET CHAR(10) DEFAULT 0 NOT NULL
);

CREATE SEQUENCE REPLY_SEQ;

COMMIT;

----------------------------------------------------------------------------------------

CREATE TABLE NOTICE (

	NUM	NUMBER(10,0) NOT NULL PRIMARY KEY,
	TITLE VARCHAR2(1000) NOT NULL,
	CONTENT	VARCHAR2(4000)  NOT NULL,
    	ID	VARCHAR2(20) NOT NULL CONSTRAINT NOTICE_ID_FK REFERENCES MEMBER(ID) ON DELETE CASCADE,
	WRITEDATE DATE DEFAULT SYSDATE,
	UPDATEDATE DATE DEFAULT SYSDATE,
   	READCOUNT NUMBER(10) DEFAULT 0
    
);


CREATE SEQUENCE NOTICE_SEQ;

commit;


----------------------------------------------------------------------------------------

CREATE TABLE QNA (

    QNUM NUMBER(10) PRIMARY KEY NOT NULL,
    ID VARCHAR2(20) CONSTRAINT QNA_ID_FK REFERENCES MEMBER(ID) NOT NULL,
    TITLE VARCHAR2(1000) NULL,
    CONTENT VARCHAR2(4000) NULL,
    WRITEDATE DATE DEFAULT SYSDATE,
    UPDATEDATE DATE DEFAULT SYSDATE,
    SECRET VARCHAR2(20) DEFAULT 'Y', -- Y: 비밀글 활성화  N:비활성화
    REPLYCNT  number(10) DEFAULT 0
    
);

CREATE SEQUENCE QNA_SEQ;
    
select * from qna;
commit;

----------------------------------------------------------------------------------------

CREATE TABLE QNAREPLY (

   RENUM NUMBER(10)   PRIMARY KEY,
   QNUM   NUMBER(10) CONSTRAINT QNAREPLY_QNUM_FK REFERENCES QNA(QNUM) ON DELETE CASCADE, -- 원 Q&A 게시글 번호
   ID   VARCHAR2(20)    CONSTRAINT QNAREPLY_ID_FK REFERENCES MEMBER(ID) ON DELETE CASCADE,
   CONTENT  VARCHAR2(4000)   NULL,
   WRITEDATE  DATE DEFAULT SYSDATE
   
);

CREATE SEQUENCE QNAREPLY_SEQ;

commit;


----------------------------------------------------------------------------------------

CREATE TABLE CART(

    CARTNUM NUMBER(10) PRIMARY KEY,
    ID VARCHAR2(20) CONSTRAINT CART_ID_FK REFERENCES MEMBER(ID) ON DELETE CASCADE,
    ISBN NUMBER(38) NULL CONSTRAINT CART_ISBN_FK REFERENCES BOOK(ISBN) ON DELETE CASCADE,
    PRODUCTNUM NUMBER(38) NULL CONSTRAINT CART_PRODUCTNUM_FK REFERENCES ALBUM(PRODUCTNUM) ON DELETE CASCADE,
    AMOUNT NUMBER(5)
    
);

CREATE SEQUENCE CART_SEQ;

commit;

----------------------------------------------------------------------------------------

CREATE TABLE BOOKIMAGE(

    ISBN NUMBER(38) CONSTRAINT BOOKIMAGE_ISBN_FK REFERENCES BOOK(ISBN) ON DELETE CASCADE,
    FILENAME VARCHAR2(2000) NOT NULL,
    UPLOADPATH VARCHAR2(1000) NOT NULL,
    UUID VARCHAR2(2000) PRIMARY KEY
    );
    

commit;

select * from bookimage;
