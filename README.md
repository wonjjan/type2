from xml.etree.ElementTree import TreeBuilder
import pygame
from pygame.locals import QUIT, KEYDOWN, K_BACKSPACE, K_RETURN
import sys
from quest import word
from random import sample
from random import randint as ri

pygame.init()

SUR = pygame.display.set_mode((500,500))
FPS = pygame.time.Clock()

class Question:
    def __init__(self):
        self.x, self.y = 0,0
        self.mess = pygame.font.SysFont(None,ri(4,9)*10)
    def setword(self):
        self.word = sample(word,1)[0]
        self.m = self.mess.render(f"{self.word}", True, (ri(0,255), ri(0,255), ri(0,255)))
        self.sx, self.sy = self.m.get_width(), self.m.get_height()
    def show(self):
        SUR.blit(self.m, (self.x, self.y))

user = pygame.font.SysFont(None,60)
smess = pygame.font.SysFont(None,60)
rmess = pygame.font.SysFont(None,60)

qlist = []
st = ""
ucnt = 0
comcnt = 0
game_over = False

while True:
    SUR.fill((255,255,255))
    for i in pygame.event.get():
        if i.type == QUIT:
            pygame.quit()
            sys.exit()
        elif i.type == KEYDOWN:
            if i.key == K_BACKSPACE:
                st = st[:-1]
            elif i.key == K_RETURN:
                for i in qlist:
                    if i.word == st:
                        del qlist[qlist.index(i)]
                        ucnt += 1        
                st = ""
            else:
                st += i.unicode
    

    if not game_over:

        if ri(1,10) == 1:
            q = Question()
            q.setword()
            q.x, q.y = ri(0, 500-q.sx), ri(0,400-q.sy)
            qlist.append(q)

        if qlist:
            if ri(1,20) == 1:
                x = ri(0, len(qlist)-1)
                del qlist[x]
                comcnt += 1


        if comcnt == 15 or ucnt == 15:
            game_over = True

        for i in qlist:
            i.show()

        s = smess.render(f"{ucnt} vs {comcnt}", True, (255,0,0))
        SUR.blit(s, 500-s.get_width()-5, 5)


        u = user.render(st,True,(0,0,0))
        ux,uy = u.get_width(), u.get_height()
        SUR.blit(u, (250-ux//2, 450-uy//2))
        pygame.draw.line(SUR, (0,0,0), (0,400), (500,400), 3)








        pygame.draw.line(SUR,(0,0,0), (0,400), (500,400), 3)
    else:
        if ucnt == 15:
            r = rmess("YOU WIN", True, (0,0,255))
        else:
            r = rmess("YOU WIN", True, (255,0,0))
        SUR.blit(r, (250- r.get_width()//2, 250 - r.get_height()//2))



    pygame.display.flip()
    FPS.tick(15)
    



